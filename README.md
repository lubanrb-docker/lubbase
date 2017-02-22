# Uber Lubbase docker image development-1.0.3

## Prerequisites

You need to install the following prerequisites before you can build and run the Lubbase image:
  * [Install Docker Engine (1.13.0 or above)](https://docs.docker.com/engine/installation/)
  * [Install Docker Compose (1.10.0 or above)](https://docs.docker.com/compose/install/)

## Build the Lubbase image

### Download the build scripts

```
cd your_work_directory
curl -sSL -o lubbase-development-1.0.3.tar.gz  https://github.com/lubanrb-docker/lubbase/archive/development-1.0.3.tar.gz
```

### Uncompress the tar ball

```
tar -xzf lubbase-development-1.0.3.tar.gz
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
cd lubbase-development-1.0.3/docker
docker-compose build
```

#### Build the image with current uid

```
cd lubbase-development-1.0.3/docker
LUBAN_UID=$(id -u $(whoami)) docker-compose build
```

You can use the same way to override other build settings temporarily.

## Run the Lubbase image

When you start a container from the image for the very first time, you need to update the user permission first as root. 

```
cd lubbase-development-1.0.3/docker
docker-compose run --rm -u root lubbase

# Inside the container, run the following command to update the user permission
# Replace with your settings if the luban user name and the luban root path are different
chown -R luban:luban /opt/luban
```

Then, you can start the container with regular user:

```
cd lubbase-development-1.0.3/docker
docker-compose run --rm lubbase
```

The user permission update is to walk around two issues in current docker version:

  * Dockerfile ADD/COPY does NOT honor USER and thus, files always are owned by root #6119
  * Inefficient layer sizes when changing permissions or adding directories to existing directories #5505

This might not be necessary in the future docker releases. 

