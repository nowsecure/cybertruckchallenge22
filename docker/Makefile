DOCKER_IMG := cbtruck

.PHONY: build run stop remove shell

build:
	docker build -t "$(DOCKER_IMG):latest" .

stop:
	docker stop $(DOCKER_IMG)

remove:
	docker image rm -f $(DOCKER_IMG)

shell:
	docker run --network host -v /tmp:/tmp -it --entrypoint bash $(DOCKER_IMG)


