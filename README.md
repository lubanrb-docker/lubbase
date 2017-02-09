# Uber Lubbase docker image development-1.0.0

## Prerequisites

You need to install the following prerequisites before you can build and run the Lubbase image:
  * [Docker Engine (1.13.0 or above)](https://docs.docker.com/engine/installation/)
  * [Install Docker Compose (1.10.0 or above)](https://docs.docker.com/compose/install/)

## Build the Lubbase image

Download the build scripts from the repository:

```
cd your_work_directory
curl -sSL -o lubbase-development-1.0.0.tar.gz  https://github.com/lubanrb-docker/lubbase/archive/development-1.0.0.tar.gz
```

Uncompress the tar ball:

```
tar -xzf lubbase-development-1.0.0.tar.gz
```

Build the image:

```
cd lubbase-development-1.0.0/docker
docker-compose build
```

## Run the Lubbase image

```
cd lubbase-development-1.0.0/docker
docker-compose run --rm lubbase
```

