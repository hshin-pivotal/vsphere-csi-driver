# Copyright 2019 The Kubernetes Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

all: build

include ../../hack/make/login-to-image-registry.mk

VERSION ?= $(shell git describe --always --dirty)
IMAGE := $(REGISTRY)/csi-ci

.PHONY: build
build:
	docker build -t $(IMAGE):$(VERSION) -f Dockerfile ../..
	docker tag $(IMAGE):$(VERSION) $(IMAGE):latest

.PHONY: push
push: login-to-image-registry
	docker push $(IMAGE):$(VERSION)
	docker push $(IMAGE):latest

.PHONY: clean
DOCKER_RMI_FLAGS := --no-prune
clean:
	docker rmi $(DOCKER_RMI_FLAGS) $(IMAGE):$(VERSION) $(IMAGE):latest 2>/dev/null || true

.PHONY: clobber
clobber: DOCKER_RMI_FLAGS :=
clobber: clean
	docker rmi $$(docker images -qf reference=$(IMAGE):*) 2>/dev/null || true

.PHONY: print
print:
	@echo $(IMAGE):$(VERSION)
