# Lab 04 - Building your own Docker images

## Prerequisites

Before procedding with this lab, please create an account on [Docker Hub](https://hub.docker.com/)

Also, before proceeding with this lab, please familiarize yourself first with the topics below:

* [Dockerfile](https://docs.docker.com/engine/reference/builder/)

## Naming conventions Docker images

Before we can proceed we need to quickly go over the Docker images naming conventions.  A Docker image consists of 3 parts:

1. the user/organization
2. the actual image name
3. a tag

An example would be:

```
gluobe/container-info:latest
```

NOTE:
- for official images, the user/organization is ommitted, for example `nging:latest` or `centos:6`
- if no tag is specified the default tag, latest, is used

## Building your first Docker image

Before we can start building Docker images we first need some code (PHP) that we will inject into our Docker image (if you know how to write PHP code you are more than welcome to use your own written code here), we will also already create an empty Dockefile.

```
cd ~
git clone https://github.com/gluobe/container-info
cd container-info
touch Dockerfile
```

Using your favorite editor (vi, nano, emacs,...), enter the following lines into the Dockerfile file.

```
FROM php:apache

COPY . /var/www/html/
```

When we build the above Dockerfile the following will happen:
1. we will start from the official PHP Docker images (with Apache included)
2. we will copy the content of the current directory of our Ubuntu host to the /var/www/html directory inside the image

To build the image run the following command:

```
docker build -t <DOCKER_HUB_USERNAME>/container-info:latest .
```

NOTE: be sure to copy/type the trailing '.'

The output should be something like:

```
root@ip-172-31-26-41:~/container-info# docker build -t test .
Sending build context to Docker daemon 433.2 kB
Step 1 : FROM php:apache
apache: Pulling from library/php

75a822cd7888: Pull complete
e4d8a4e038be: Pull complete
81d4d961577a: Pull complete
f0a3d7c702e3: Pull complete
a4b7d2c4c9cc: Pull complete
de3fbbff60a9: Pull complete
336c38203cc2: Pull complete
628c443fd26f: Pull complete
488d6737080b: Pull complete
09922121d03f: Pull complete
ca2cc6ada1a6: Pull complete
b92d31559887: Pull complete
5f6ab278e229: Pull complete
Digest: sha256:d5373e9b14da5058a19ca0cf6409286a666865be5574a7fa69290e16cdf4aeef
Status: Downloaded newer image for php:apache
 ---> 62001e0da76d
Step 2 : COPY . /var/www/html/
 ---> 6b9a89afe866
Removing intermediate container 31c3691d85da
Successfully built 6b9a89afe866
```

Check with `docker images` that your own image is indeed locally avialable:

```
root@ip-172-31-26-41:~/container-info# docker images
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
gluobe/container-info   latest              6b9a89afe866        13 minutes ago      386.1 MB
```

Ensure that no other container is running `docker stop $(docker ps -qa)` and start a container from your freshly baked Docker image:

```
docker run -d -p 80:80 <DOCKER_HUB_USERNAME>/container-info:latest
```

Surf to http://nodeXY.<PROJECT_NAME>.gluo.io to see if everything is working.

## Publishing you Docker image

At the moment your image is only available locally, to make it publically available you will need to login to Docker Hub and push you image.

Login to Docker Hub

```
docker login
```

Push your image to Docker Hub:

```
docker push <DOCKER_HUB_USERNAME>/container-info:latest
```

Now your image is publically available on the Docker Hub, so everybody can start using your image (try to run an image from another team).
