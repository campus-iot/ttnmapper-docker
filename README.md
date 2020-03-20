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

build "ttnmapper-web-v2" "latest"
build "ttnmapper-api-v2" "latest"
build "ingress-api" "latest"

docker images | grep ttnmapper
```

## Pulling the dependency

RabbitMQ
```bash
docker pull TBD
```

Database (which one ?)
```bash
docker pull TBD
```

Nodered
```bash
docker pull TBD
```

## Running the TTNMapper container

```bash
TBD
```


