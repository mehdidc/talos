SHELL := /bin/bash

GIT_SHORT_SHA = $(shell git rev-parse --short=7 HEAD)
GIT_DIRTY = $(shell [[ -n `git status -s` ]] && echo -dirty)
OS_IMAGE_NAME ?= autonomy/talos-os
TAG = $(GIT_SHORT_SHA)$(GIT_DIRTY)
DOCKER_COMPOSE ?= IMAGE=$(OS_IMAGE_NAME) TAG=$(TAG) docker-compose
IP_ADDR ?= 10.5.0.6

export TALOSCONFIG = $(PWD)/talosconfig

all: up

.PHONY: up
up: talosconfig
	@$(DOCKER_COMPOSE) up -d $(SERVICES)

.PHONY: down
down:
	@$(DOCKER_COMPOSE) down -v $(SERVICES)

talosconfig:
	@mkdir -p pki
	@./gen.sh $(IP_ADDR)

kubeconfig:
	@$(DOCKER_COMPOSE) run --rm osctl kubeconfig > kubeconfig

.PHONY: clean
clean: down
	-@rm talosconfig
	-@rm kubeconfig
	-@rm userdata/master-1.yaml
	-@rm userdata/master-2.yaml
	-@rm userdata/master-3.yaml
	-@rm userdata/worker-1.yaml
	-@rm -rf pki
	-@rm -rf logs
