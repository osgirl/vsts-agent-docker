DOCKER_ORG_PREFIX = emisgroup
DOCKER_IMAGE_NAME = vsts-agent-docker
DOCKER_TAG = ubuntu-16.04-php-node

.PHONY: explain
explain:
	### Welcome
	#
	# Makefile for building linux vsts agents with php and node installed
	#
	### Available to you
	#
	# -> $$ make build
	# -> $$ make tag
	# -> $$ make push
	#
	# Development mode
	#
	# -> $$ VSTS_DEV=true make run
	#
	# Production mode
	#
	# -> $$ make run

.PHONY:build
build:
	docker build -t $(DOCKER_ORG_PREFIX)/$(DOCKER_IMAGE_NAME) .

.PHONY:tag
tag:
	docker tag $(DOCKER_ORG_PREFIX)/$(DOCKER_IMAGE_NAME) $(DOCKER_ORG_PREFIX)/$(DOCKER_IMAGE_NAME):$(DOCKER_TAG)

.PHONY:push
push:
	docker push $(DOCKER_ORG_PREFIX)/$(DOCKER_IMAGE_NAME):$(DOCKER_TAG)

.PHONY:pull
pull:
	docker pull $(DOCKER_ORG_PREFIX)/$(DOCKER_IMAGE_NAME):$(DOCKER_TAG)

.PHONY:run
run:
ifeq ($(VSTS_DEV),true)
	$(eval VSTS_RUN_MODE=-it)
else
	$(eval VSTS_RUN_MODE=-d)
endif

	docker run \
		-e VSTS_ACCOUNT \
		-e VSTS_POOL \
		-e VSTS_TOKEN \
		-e VSTS_AGENT='$(hostname)-docker-agent' \
		-v /var/run/docker.sock:/var/run/docker.sock \
		-v ${HOME}/.ssh:/root/.ssh:ro \
		${VSTS_RUN_MODE} \
		$(DOCKER_ORG_PREFIX)/$(DOCKER_IMAGE_NAME)
