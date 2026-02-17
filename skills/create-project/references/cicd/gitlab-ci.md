# GitLab CI Reference

## .gitlab-ci.yml

```yaml
image: oven/bun:latest

stages:
  - validate
  - build

cache:
  paths:
    - node_modules/

install:
  stage: validate
  script:
    - bun install --frozen-lockfile

lint:
  stage: validate
  script:
    - bun run lint

typecheck:
  stage: validate
  script:
    - bunx tsc --noEmit

test:
  stage: validate
  script:
    - bun test

build:
  stage: build
  script:
    - bun run build
```

## With Docker Build

Add a deploy stage if Docker is enabled:

```yaml
  - deploy

docker:
  stage: deploy
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA .
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
```
