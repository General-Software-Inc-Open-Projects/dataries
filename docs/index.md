# DATARIES

Dataries is a product that allows the ingestion, processing, analysis and visualization of large volumes of data from different sources.

## Dataries architecture 

Dataries has a component-based architecture where its own developments are interrelated with established and recognized technologies in the big data field.

These components allow the management and configuration of data flows, the management of the traces of all the components of the system, the check of the availability of the elements of the system, as well as the storage and distributed processing of the information.

![architecture](./img/0.png)

## Dataries deployment

Dataries can be deployed in a distributed way in different clouds or on local servers since its deployment is based on Docker containers.

![deployment](./img/1.png)

## Dataries open source

Dataries is now an open source product, to encourage the contribution and collaboration of other developers. For this we have used GitHub and Docker Hub. Here is a list of all our docker images that you can download from our `gsiopen` organization: 

- gsiopen/hadoop
- gsiopen/config-server
- gsiopen/schema-registry
- gsiopen/eureka
- gsiopen/zuul-server
- gsiopen/zookeeper
- gsiopen/pulsar
- gsiopen/zuul-client

Each of this docker image brings with it the necessary documentation to put it to work independently, however, to start Dataries, other configurations are needed.

### How to connect Dataries

Dataries can be deployed by using these docker compose:

compose 1

```
version: "3.7"
networks:
  dataries-net:
    name: dataries-net
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 192.168.99.0/24

services:
 postgresql:
    image: postgres
    container_name: postgresql
    hostname: postgresql
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=postgres
    volumes:
      - ./dataries-postgresql:/docker-entrypoint-initdb.d
    restart: on-failure
    ports: ["5432:5432"]
    expose: ["5432"]
    networks:
      dataries-net:
        ipv4_address: 192.168.99.12
```

compose 2

```
```