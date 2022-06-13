DOCKER_IMG := cbtruck

.PHONY: build build-emu stop remove shell run shell-emu

build:
	docker build -t "$(DOCKER_IMG):latest" .

build-emu:
	docker pull androidsdk/android-30

stop:
	docker stop $(DOCKER_IMG)

remove:
	docker image rm -f $(DOCKER_IMG)

shell run:
	docker run --network host -v /tmp:/tmp -it --entrypoint bash $(DOCKER_IMG)

shell-emu:
	docker run -it --rm  -v /tmp:/tmp --network host --device /dev/kvm androidsdk/android-30:latest bash

