# Usage

This directory includes two Dockerfiles:
* `Dockerfile` is cannonical one used to generate the Docker Hub official 
image. It downloads GeoNetwork 4.0.0-alpha.1 WAR file from sourceforge.  
* `Dockerfile.local` needs a `geonetwork.war` file next to it to build
the image.

It also includes two docker-compose configuration files.
* `docker-compose.yml` uses official GeoNetwork image from Docker Hub.
* `docker-compose.dev.yml` can be applied to override the image used in 
`docker-compose.yml` and build the GeoNetwork image using `Dockerfile.local`.



## Pre-built image
To use the pre-built image you can use the `docker-compose.yml` file provided 
in this directory:

```shell
docker-compose up 
```

## Local image

To be able to generate a elasticsearch-ready docker image, you will have:

1. Build your geonetwork.war (https://geonetwork-opensource.org/manuals/trunk/en/maintainer-guide/installing/installing-from-source-code.html#the-quick-way)
2. Clone this repository

```shell
git clone https://github.com/geonetwork/docker-geonetwork.git
cd docker-geonetwork/4.0.0-alpha.1
```

3. Get the generated webapp in the current directory, name it `geonetwork.war`

```shell
cp ../../core-geonetwork/web/target/geonetwork.war .
```

4. Run the docker-composition from the current directory:

```shell
docker-compose -f docker-compose.yml -f docker-compose.dev.yml docker-compose up --build
```

5. Open http://localhost:8080/geonetwork/ in a browser

## Database configuration

The provided docker-compose shows an example of using GeoNetwork with an
external database (based on the `mdillon/postgis` docker image). If the
environment variables related to the database from the `geonetwork` service are
not defined, then the geonetwork will launch with an integrated h2 database.

As soon as the `POSTGRES_DB_HOST` variable is defined on the `geonetwork`
service, the docker entrypoint will reconfigure at runtime to target this type
of database, and it will require the other related env variables to be defined
as well (username, password, db name and so on).

