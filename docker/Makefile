DOCKER_IMG := cbtruck

.PHONY: build build-local build-emu build-emu-local stop remove shell run shell-emu

build:
	docker build -t "$(DOCKER_IMG):latest" .

build-local:
	docker pull cybertruck2:5000/cbtruck

build-emu:
	docker pull androidsdk/android-30

build-emu-local:
	docker pull cybertruck2:5000/androidemu

stop:
	docker stop $(DOCKER_IMG)

remove:
	docker image rm -f $(DOCKER_IMG)

shell run:
	docker run --network host -v /tmp:/tmp -it --entrypoint bash $(DOCKER_IMG)

shell-emu:
	docker run -it --rm  -v /tmp:/tmp --network host --device /dev/kvm androidsdk/android-30:latest bash

