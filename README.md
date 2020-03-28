# Base docker4laravel

Base docker containers for Laravel projects.

## Using it

First clone this repository in your project root directory:

```
git clone git@github.com:mastercode-deb/base-docker4laravel.git docker
```

We don't recommend you to track this project inside yours, so remove `.git` repository:

```
cd docker && rm -rf .git
```

Create a `.env` file based on `env-example` and edit it to your needs.

> Don't forget to change the COMPOSE_PROJECT_NAME to your project name.

You can also edit `docker-compose.yml` to add new containers or remove those you don't need.

## Running containers

You can use the official `docker-compose` commands, but this repository includes a simpler and shorter command to run basic container commands:

*Initialize containers:*

```
./dc up
```

*Access shell:*

```
./dc sh
```

*Stop containers:*

```
./dc stop
```

## Composer and npm/yarn

It's highly recommended to install composer and node packages from the php-fpm container shell. To do so first run `./dc sh`, then you can use composer and node tools as you already know.

## Containers

- nginx
- php-fpm
- postgresql
- redis
- mongo
- elasticsearch
- graylog

> If you run Graylog container, make sure to include mongo and elasticsearch as well.
