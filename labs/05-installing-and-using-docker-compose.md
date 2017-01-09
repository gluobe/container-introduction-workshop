# Lab 05 - Installing and using docker-compose

## Prerequisites

Before proceeding with this lab, please familiarize yourself first with the topics below:

* [docker-compose](https://docs.docker.com/engine/reference/glossary/#compose)

## Installing docker-compose

Installing docker-compose is easy, simply run the following commands:

```
curl -L https://github.com/docker/compose/releases/download/1.9.0/docker-compose-Linux-x86_64 > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

To verify that `docker-compose` is correctly installed run the `docker-compose version` command, you should seem something similar to:

```
docker-compose version 1.9.0, build 2585387
docker-py version: 1.10.6
CPython version: 2.7.9
OpenSSL version: OpenSSL 1.0.1t  3 May 2016
```
