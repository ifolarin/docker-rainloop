NAME = webbasedsol/rainloop
VERSION = 0.3.2

.PHONY: all build build-nocache test tag_latest release

all: build

build:
	docker build -t $(NAME):$(VERSION) --rm .

build-nocache:
	docker build -t $(NAME):$(VERSION) --no-cache --rm .

test:
	env NAME=$(NAME) VERSION=$(VERSION) bats test/test.bats

tag_latest:
	docker tag $(NAME):$(VERSION) $(NAME):latest

release: build test tag_latest
	@if ! docker .s $(NAME) | awk '{ print $$2 }' | grep -q -F $(VERSION); then echo "$(NAME) version $(VERSION) is not yet built. Please run 'make build'"; false; fi
	docker push $(NAME)
	@echo "*** Don't forget to run 'twgit release/hotfix finish' :)"
