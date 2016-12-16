# PostgreSQL 9.6 Dockerfile on CentOS 7

This repository contains a Dockerfile to build a Docker Image for PostgreSQL on CentOS 7

## Base Docker Image

* [centos](https://hub.docker.com/_/centos/)

## Usage

### Installation

1. Install [Docker](https://www.docker.com/).

``` build from Github ```

To create an image, clone this repository and execute the following command on the docker-postgresql folder:

`docker build -t myname/postgresql:latest .`

Another alternatively, you can build an image directly from Github:

`docker build -t="myname/postgresql:latest" github.com/kokdemir/docker-postgresql`

### Create and running a container

**Create container:**

(Not recommended for production use)

``` docker create -it -p 5432:5432 --name mypg96 myname/postgresql ```

**Start container:**

``` docker start postgresql96 ```


**Another way to start a postgresql container:**

``` docker run -d -p 5432:5432 --name mypg96 myname/postgresql ```

### Connection methods:

**PostgreSQL client:**

`docker exec -it mypg96 psql`

**Bash:**

`docker exec -it mypg96 bash`


### Creating a database and username

You can create a postgresql database and superuser at launch. Use `DB_NAME`, `DB_USER` and `DB_PASS` variables.

```
docker create -it -p 5432:5432 --name mypg96 --env 'DB_USER=YOUR_USERNAME' --env 'DB_PASS=YOUR_PASSWORD' --env 'DB_NAME=YOUR_DATABASE' myname/postgresql

```
 
If you don't set DB_PASS variable, an automatic password is generated for the PostgreSQL database user. Check to stdout/stderr log of container created:

```
docker run -d -p 5432:5432 --name mypg96 --env 'DB_USER=YOUR_USERNAME' --env 'DB_NAME=YOUR_DATABASE' myname/postgresql
docker logs mypg96
```

The output:

```
...
WARNING: 
No password specified for "YOUR_USERNAME". Generating one
Password for "YOUR_USERNAME" created as: "aich3aaH0yiu"
...
```

To connect to newly created postgresql container:

`docker exec -it mypg96 psql -U YOUR_USERNAME`

Another way to connect to postgresql container with your newly created user:

```
psql -U YOUR_USERNAME -h $(docker inspect --format {{.NetworkSettings.IPAddress}} postgresql94)
```


### Upgrading

Stop the currently running image:

``` docker stop mypg96 ```
