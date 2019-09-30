
# Use this to control building and pushing of Docker images in a significantly less ugly fashion.
#
push-all:
	$(push-server-cmd)
	$(push-daemons-cmd)
	$(push-cache-cmd)
	$(push-messenger-cmd)

build-all:
	$(build-server-base-cmd)
	$(build-server-cmd)
	$(build-daemons-base-cmd)
	$(build-daemons-cmd)
	$(build-cache-cmd)
	$(build-messenger-cmd)

build-all-nc:
	$(build-server-base-nc-cmd)
	$(build-server-nc-cmd)
	$(build-daemons-base-nc-cmd)
	$(build-daemons-nc-cmd)
	$(build-cache-nc-cmd)
	$(build-messenger-nc-cmd)

define build-server-base-cmd =
docker build -t rucio-server-base server-base
endef

define build-server-base-nc-cmd =
docker build --no-cache -t rucio-server-base server-base
endef

define build-server-cmd =
docker build -t rucio-server server	
endef

define build-server-nc-cmd =
docker build --no-cache -t rucio-server server	
endef

define build-daemons-base-cmd =
docker build -t rucio-daemons-base daemons-base
endef

define build-daemons-base-nc-cmd =
docker build --no-cache -t rucio-daemons-base daemons-base
endef

define build-daemons-cmd =
docker build -t rucio-daemons daemons
endef

define build-daemons-nc-cmd =
docker build --no-cache -t rucio-daemons daemons
endef

define build-cache-cmd =
docker build -t rucio-cache cache
endef

define build-cache-nc-cmd =
docker build --no-cache -t rucio-cache cache
endef

define build-messenger-cmd =
docker build -t rucio-messenger messenger
endef

define build-messenger-nc-cmd =
docker build --no-cache -t rucio-messenger messenger
endef

build-server-base:
	$(build-server-base-cmd)

build-server-base-nc:
	$(build-server-base-nc-cmd)	

build-server:
	$(build-server-cmd)

build-server-nc:
	$(build-server-nc-cmd)

build-daemons-base:
	$(build-daemons-base-cmd)

build-daemons-base-nc:
	$(build-daemons-base-nc-cmd)

build-daemons:
	$(build-daemons-cmd)

build-daemons-nc:
	$(build-daemons-nc-cmd)

build-cache:
	$(build-cache-cmd)	

build-cache-nc:
	$(build-cache-nc-cmd)	

build-messenger:
	$(build-messenger-cmd)
	
build-messenger-nc:
	$(build-messenger-nc-cmd)	

define push-server-cmd =
	docker tag rucio-server bjwhitefnal/fnal-rucio-server
	docker push bjwhitefnal/fnal-rucio-server
endef

define push-daemons-cmd =
	docker tag rucio-daemons bjwhitefnal/fnal-rucio-daemons
	docker push bjwhitefnal/fnal-rucio-daemons
endef

define push-cache-cmd =
	docker tag rucio-cache bjwhitefnal/fnal-rucio-cache
	docker push bjwhitefnal/fnal-rucio-cache
endef

define push-messenger-cmd =
	docker tag rucio-messenger bjwhitefnal/fnal-rucio-messenger
	docker push bjwhitefnal/fnal-rucio-messenger
endef

push-server:
	$(push-server-cmd)

push-daemons:
	$(push-daemons-cmd)

push-cache:
	$(push-cache-cmd)

push-messenger:
	$(push-messenger-cmd)