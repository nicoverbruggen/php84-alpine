# php84-alpine

## What is this?

This is a custom build based on PHP 8.4's Alpine docker image, with changes to make Laravel back-end testing easily possible.

This image includes:

- PHP 8.4 with `bcmath`, `exif`, `gd`, `intl`, `mysqli`, `opcache`, `pcntl`, `pdo_mysql`, `pdo_pgsql`, `pgsql`, `sodium`, `xdebug`, `zip` and `imagick` installed
- Packages: `curl`, `git`, `sqlite`, `nano`, `ncdu`, `nodejs`, `npm`
- Composer also comes pre-installed

For the latest list of inclusion, see the [Dockerfile](./Dockerfile).

## Quick start

In order to build and then test the container:

    docker buildx build . --platform linux/amd64 -t nicoverbruggen/php84-alpine \
    && docker run -it nicoverbruggen/php84-alpine sh

You may omit the `--platform` flag if you wish to build a container for your own architecture, but there may be issues with dependencies.

## Automatic builds

The automatically build the container and have it pushed, you must:

* Tag the commit you wish to build
* Create a new release with said tag

The Docker action will automatically build the release and push it under that tag to Docker Hub.

## Example usage

`.gitlab-ci.yml`
```
tests:
  only:
    - main
  image: nicoverbruggen/php84-alpine:latest
  script:
    - cp .env.ci .env
    - cp .env.ci .env.testing
    - composer install
    - npm install --silent
    - npm run production
    - touch ./database/tests.sqlite
    - vendor/bin/pest --coverage --colors=never
```

## Where can I find it?

You can find the image on Docker Hub here: https://hub.docker.com/r/nicoverbruggen/php84-alpine.