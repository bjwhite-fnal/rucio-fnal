
# Fermilab Rucio OKD Cluster Deployment 
## Scientific Data Management
### Brandon White
bjwhite@fnal.gov

Use this to deploy Rucio for an experiment. First source an experiments environment file and deploy with the experiment generic rucio-fnal framework.
In particular please make sure that the following are set by the sourcing of the environment file:
~~~~
    EXPERIMENT: Name of the experiment that configuration files are being generated for. Used when generating helm deployment configurations.
    FNAL_RUCIO_DIR: Set to the top level directory of the rucio-fnal repository (i.e. the directory this README resides in). Enables configuration files to be found in the directory structure detailed below.
    FNAL_EXP_RUCIO_CERT: Certificate for service authentication.
    FNAL_EXP_RUCIO_KEY: Key for service authentication.
    FNAL_EXP_RUCIO_CA_BUNDLE: CA certificates for service authentication.
    FNAL_EXP_RUCIO_CERT_KEY_COMBINED: Certificate and key concatenated. Needed for the Web UI.
    FNAL_RUCIO_VERSION: Use this when building fnal-rucio-* images to select the desired Rucio version for installation into the image. 
~~~~

## Before Deployment To-Do List
~~~~
    0. Have a database created for the new deployment to use. (Duh)
    1. Create rucio-<experiment> project in OKD
    2. Make sure you have the certificate and key for the Rucio service, with alternate names for the various services (webui, msg, auth)
    3. Ensure that the certificate is registered in a federated identity system (e.g. FERRY/VOMS) to the <experiment>pro user so that Rucio can access data on the RSEs
    4. Edit <experiment>/setup_rucio_env.sh to set the required environment variables
    5. Have the FNAL OKD cluster administrators create the `useroot` account for the deployment to use in order to allow the containers to run as root
    6. Have the FNAL OKD cluster administrators create the following DNS records:
        <experiment>-rucio.okd.fnal.gov
        auth-<experiment>-rucio.okd.fnal.gov
        msg-<experiment>-rucio.okd.fnal.gov
        webui-<experiment>-rucio.okd.fnal.gov
~~~~

Source the setup_rucio_env.sh file at the top of the given experiment's configuration tree to configure the environment.
Use deploy-rucio.sh to deploy services onto the OKD cluster after the environment is configured appropriately as described above.
Use undeploy-rucio.sh to remove everything (Services, Pods, Routes, Persistent Volumes and Claims, etc..).

The Rucio source code may be patched when the images are built by placing patch files into one of two locations: $FNAL_RUCIO_DIR/rucio-fnal/docker/[server,daemon]/patches

## EXAMPLE DIRECTORY STRUCTURE FOR AN EXPERIMENT:
~~~~
    dune
    ├── certs                               < Experiment specific certificate, key, cert/key concatenated, and CA certificate bundle. >
    │   ├── ca_bundle.pem
    │   ├── rucio.fnal.gov_cert.pem
    │   └── rucio.fnal.gov_key.pem
    ├── helm
    │   ├── cache
    │   │   └── values.yaml
    │   ├── filebeat 
    │   │   └── values.yaml
    │   ├── logstash 
    │   │   └── values.yaml
    │   ├── daemons
    │   │   └── values.yaml                 < Daemon configuration specific values > 
    │   ├── messenger
    │   │   └── values.yaml
    │   ├── osg_authentication
    │   │   └── values.yaml
    │   ├── secret
    │   │   └── config.yaml                 < Secret information such as DB connection string. >
    │   ├── server
    │   │   └── values.yaml                 < Server configuration specific values >
    │   └── webui
    │       └── values.yaml
    ├── patches
    └── setup_dune_rucio_env.sh             < Export the needed environment variables for experiment setup >
~~~~

Directories named certs/ and secret/ will not be committed to the repository according to the default .gitignore file.

When building new container images, setup the the FNAL OKD Rucio environment. Use hard links to link the permissions-fnal/ Python files into the server, daemons, and WebUI Docker build contexts via a /permissions in each directory.
Use the Makefile in fnal-rucio/docker to build and push Docker images.

## Using custom Policy Packages
Rucio policy package directories must be added to the PYTHONPATH search variable inside of the Rucio containers. (See https://github.com/rucio/rucio/blob/master/doc/source/policy_packages.rst for details) 
Make sure to set the environment variable `policy_pkg_dir` in the containers (using the optional_config section in the Helm values files of the server/daemon/webui/etc... If this variable is set, the value will be prepended
to the value of PYTHONPATH inside the container.

Finally, make sure to specify under the `config` section the following settings for policies:
```
policy:
  package: fermilab
  support: "Brandon White <bjwhite@fnal.gov>"
```
Do NOT specify the permission and schema options... This prevents the correct policy from being imported.

## Note about Reaper certificates
The Reaper needs a full directory of certificats as opposed to a concatenated PEM. This should be handled by this framework, but it is worth keeping in mind.
