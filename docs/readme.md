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

- [gsiopen/hadoop](https://hub.docker.com/repository/docker/gsiopen/hadoop)
- [gsiopen/config-server](https://hub.docker.com/repository/docker/gsiopen/config-server)
- [gsiopen/schema-registry](https://hub.docker.com/repository/docker/gsiopen/schema-registry)
- [gsiopen/eureka](https://hub.docker.com/repository/docker/gsiopen/eureka)
- [gsiopen/zuul-server](https://hub.docker.com/repository/docker/gsiopen/zuul-server)
- [gsiopen/zookeeper](https://hub.docker.com/repository/docker/gsiopen/zookeeper)
- [gsiopen/pulsar](https://hub.docker.com/repository/docker/gsiopen/pulsar)
- [gsiopen/zuul-client](https://hub.docker.com/repository/docker/gsiopen/zuul-client)

Each of this docker image brings with it the necessary documentation to put it to work independently, however, to start Dataries, other configurations are needed.

### How to connect Dataries

Dataries can be deployed by using these docker compose:

`compose 1`

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
    expose: ["5432"]
    networks:
      dataries-net:
        ipv4_address: 192.168.99.12
  git-server:
    image: gitea
    container_name: git-server
    hostname: git-server
    environment:
     - ENV SERVICE_NAME=git-server
     - ENV USER_UID=1000
     - ENV USER_GID=1000
     - ENV APP_NAME="Git Server"
     - ENV HTTP_PORT=3000
     - ENV DB_TYPE=postgres
     - ENV DB_HOST=postgresql:5432
     - ENV DB_NAME=gitea
     - ENV DB_USER=gitea
     - ENV DB_PASSWD=gitea
     - ENV SSH_DOMAIN=git-server
     - ENV INSTALL_LOCK=true
    volumes:
      - ./dataries-gitea/app.ini:/data/gitea/conf/app.ini
    restart: on-failure
    extra_hosts: ["postgresql-server:192.168.99.12"]
    ports: ["127.0.0.1:2222:22", "3000:3000"]
    networks:
      dataries-net:
        ipv4_address: 192.168.99.13
    depends_on:
      - postgresql
```

`compose 2`

```
```