# TTNMapper Docker

## Introduction
TTNMapper is a dataviz service for the radio coverage of the public TTN LoRaWAN network.

The goal of this project is running a local instance of TTNMapper and NodeRED flow for collecting messages from Chirpstack private network.

## Building of TTNMapper Docker Images

The repositories are here https://github.com/ttnmapper

```bash
cd github/
mkdir ttnmapper
cd ttnmapper/

build() {
    REPO=$1
    VERSION=$2
    git clone https://github.com/ttnmapper/$REPO.git
    (cd $REPO; docker build -t ttnmapper/$REPO:$VERSION .)
    docker images | grep ttnmapper/$REPO
}


# TODO configure conf.json according to https://github.com/ttnmapper/mysql-insert-raw/blob/master/conf.json.template
build "mysql-insert-raw" "latest"

# TODO configure conf.json according to TBD
build "ttnmapper-web-v2" "latest"

# TODO configure conf.json according to TBD
build "ttnmapper-api-v2" "latest"

# TODO configure conf.json according to https://github.com/ttnmapper/ingress-api#configuration
build "ingress-api" "latest"

docker images | grep ttnmapper
```

## Pulling the dependencies

[RabbitMQ](https://hub.docker.com/_/rabbitmq/)
```bash
docker pull rabbitmq
```

[MySQL](https://hub.docker.com/_/mysql/)
```bash
docker pull mysql
```

[PHPMyAdmin](https://hub.docker.com/r/phpmyadmin/phpmyadmin) : optional
```bash
docker pull docker run --name myadmin -d -e PMA_HOST=dbhost -p 8080:80 phpmyadmin/phpmyadmin

```

[Nodered](https://hub.docker.com/r/nodered/node-red)
```bash
docker pull nodered/node-red
```

## Running the containers

[RabbitMQ](https://hub.docker.com/_/rabbitmq/)
```bash
VERSION=latest
IMAGE=rabbit
SERVICE=ttnmapper-$IMAGE
docker run -d --hostname $SERVICE --name $SERVICE $IMAGE:$VERSION
docker logs -f $SERVICE
```

[MySQL](https://hub.docker.com/_/mysql/)
```bash
VERSION=latest
IMAGE=mysql
SERVICE=ttnmapper-$IMAGE
ENVIR="-e MYSQL_ROOT_PASSWORD=my-secret-pw"
docker run -d $ENVIR --hostname $SERVICE --name $SERVICE $IMAGE:$VERSION
docker logs -f $SERVICE
```

```bash
VERSION=latest
IMAGE=phpmyadmin/phpmyadmin
SERVICE=ttnmapper-myadmin
docker run -d --name myadmin -d -e PMA_HOST=localhost -p 8080:80 $IMAGE
docker logs -f $SERVICE
```

```bash
VERSION=latest
IMAGE=ttnmapper-web-v2
SERVICE=$IMAGE
docker run -d --hostname $SERVICE --name $SERVICE $IMAGE:$VERSION
docker logs -f $SERVICE
```

```bash
VERSION=latest
IMAGE=ttnmapper-api-v2
SERVICE=$IMAGE
docker run -d --hostname $SERVICE --name $SERVICE $IMAGE:$VERSION
docker logs -f $SERVICE
```

```bash
VERSION=latest
IMAGE=ingress-api
SERVICE=$IMAGE
docker run -d --hostname $SERVICE --name $SERVICE $IMAGE:$VERSION
docker logs -f $SERVICE
```


```bash
VERSION=latest
IMAGE=nodered/node-red
SERVICE=ttnmapper-nodered
docker run -d --name $SERVICE -d -e PMA_HOST=localhost -p 8080:80 $IMAGE:$VERSION
docker logs -f $SERVICE
```
open http://localhost:3000


## Alternative: Running the containers composition

```bash
docker-compose up -d
docker-compose logs -f
```


## Production

TODO

* Add NGinx and Let'Encrypt containers
