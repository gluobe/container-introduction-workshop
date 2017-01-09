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

## Using docker-compose

Before we start with docker-compose, let us first stop all running containers:

```
docker stop $(docker ps -qa)
```

Clone the voting-app repository:

```
cd ~
git clone https://github.com/docker/example-voting-app.git
```

The example-voting-app stack consists of:

1. python application
2. nodejs applciation
3. .NET application
4. Redis
5. PostgreSQL

The entire stack can be started using a very simple command:

```
cd example-voting-app
docker-compose up
```

Test the app:
- Voting: http://nodeXY.<PROJECT_NAME>.gluo.io:5000/
- Result: http://nodeXY.<PROJECT_NAME>.gluo.io:5001/
