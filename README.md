# Uber Lubbase docker image development-1.0.2

## Prerequisites

You need to install the following prerequisites before you can build and run the Lubbase image:
  * [Install Docker Engine (1.13.0 or above)](https://docs.docker.com/engine/installation/)
  * [Install Docker Compose (1.10.0 or above)](https://docs.docker.com/compose/install/)

## Build the Lubbase image

### Download the build scripts

```
cd your_work_directory
curl -sSL -o lubbase-development-1.0.2.tar.gz  https://github.com/lubanrb-docker/lubbase/archive/development-1.0.2.tar.gz
```

### Uncompress the tar ball

```
tar -xzf lubbase-development-1.0.2.tar.gz
```

### Configure the build

The following are the build settings controlled through the dotenv(.env) file used by docker-compose:
  * DOCKER_ENGINE_VERSION
    * Docker engine version to install in the Lubbase image
  * DOCKER_COMPOSE_VERSION
    * Docker compose version to install in the Lubbase image
  * LUBAN_UID/LUBAN_USER
    * User name and ID for the non-root user to create in the Lubbase image
    * Default is 1000/luban
    * It is recommended to set the luban uid to be the same as your actual deployment user ID in the docker host, which will help your avoid any permission issues when using docker's data volumes
  * LUBAN_ROOT_PATH
    * Root path for Luban deployments
    * Default is /opt/luban

It is highly recommended to review the build settings in dotenv file before building the image.

### Build the image

Here are some commeon build instructions for your reference.

#### Build the image with the settings in dotenv:

```
cd lubbase-development-1.0.2/docker
docker-compose build
```

#### Build the image with current uid

```
cd lubbase-development-1.0.2/docker
LUBAN_UID=$(id -u $(whoami)) docker-compose build
```

You can use the same way to override other build settings temporarily.

## Run the Lubbase image

```
cd lubbase-development-1.0.2/docker
docker-compose run --rm lubbase
```

