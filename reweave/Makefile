IMAGE_VERSION ?= 2.8.4
REGISTRY_USER ?= rajchaudhuri

ALPINE_BASEIMAGE ?= alpine:3.19.1

# Targets
.PHONY: build-images
build-images: submodule
	REGISTRY_USER=${REGISTRY_USER} IMAGE_VERSION=${IMAGE_VERSION} \
	PLATFORMS="$$(docker version -f '{{ .Server.Arch }}')" \
	ALPINE_BASEIMAGE=$(ALPINE_BASEIMAGE) \
	PUBLISH=false \
	tools/build-images.sh

.PHONY: build-plugin
build-plugin: ../prog/net-plugin/launch.sh ../prog/net-plugin/config.json
	REGISTRY_USER=${REGISTRY_USER} IMAGE_VERSION=${IMAGE_VERSION} \
	tools/build-plugin.sh

.PHONY: build
build: build-images build-plugin

.PHONY: publish-images
publish-images: submodule
	REGISTRY_USER=${REGISTRY_USER} IMAGE_VERSION=${IMAGE_VERSION} \
	ALPINE_BASEIMAGE=$(ALPINE_BASEIMAGE) \
	PUBLISH=true \
	tools/build-images.sh

.PHONY: publish-plugin
publish-plugin:
	REGISTRY_USER=${REGISTRY_USER} IMAGE_VERSION=${IMAGE_VERSION} \
	PUBLISH=true \
	tools/build-plugin.sh

.PHONY: publish
publish: publish-images publish-plugin

.PHONY: clean-images
clean-images:
	REGISTRY_USER=${REGISTRY_USER} IMAGE_VERSION=${IMAGE_VERSION} \
	tools/clean-images.sh

.PHONY: clean-plugin
clean-plugin:
	REGISTRY_USER=${REGISTRY_USER} IMAGE_VERSION=${IMAGE_VERSION} \
	tools/clean-plugin.sh

.PHONY: clean
clean: clean-images clean-plugin

.PHONY: submodule
submodule: ../tools/.git

../tools/.git:
	git submodule update --init

.PHONY: scan
scan:
	REGISTRY_USER=${REGISTRY_USER} IMAGE_VERSION=${IMAGE_VERSION} \
	tools/scan-images.sh

.PHONY: clean-scan
clean-scan:
	tools/clean-scans.sh
