PACKAGE := {{ cookiecutter.package }}
COMMIT_ID := $(shell git rev-parse HEAD)
VERSION :=
BUILD_ARGS := -ldflags "-X $(PACKAGE)/consts.Version=$(VERSION) -X $(PACKAGE)/consts.CommitID=$(COMMIT_ID)"
EXTRA_BUILD_ARGS =
OUTPUT_FILE := {{ cookiecutter.package }}
IMAGE_NAME := {%- if cookiecutter.image_name %}{{ cookiecutter.image_name }}{% else %}{{ cookiecutter.package }}{% endif %}
IMAGE_TAG := latest
MAIN_FILE := main.go

.PHONY: fotmat build check-style build-image

format:
	@find $(CURDIR) -mindepth 1 -maxdepth 1 -type d -not -name vendor -not -name .git -print0 | xargs -0 gofmt -s -w
	@find $(CURDIR) -maxdepth 1 -type f -name '*.go' -print0 | xargs -0 gofmt -s -w

build:
	go build $(BUILD_ARGS) $(EXTRA_BUILD_ARGS) -o $(OUTPUT_FILE) $(MAIN_FILE)

check-style:
	@gometalinter.v2 --disable-all -E gofmt --vendor ./...

build-image:
	@docker build -t $(IMAGE_NAME):$(IMAGE_TAG) . \
	--build-arg COMMIT_ID=$(COMMIT_ID) --build-arg VERSION=$(VERSION) \
	--build-arg EXTRA_BUILD_ARGS=$(EXTRA_BUILD_ARGS)

install-linter:
	@go get -u gopkg.in/alecthomas/gometalinter.v2
