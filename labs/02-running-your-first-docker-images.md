# Lab 02 - Running your first Docker images

## Introduction

In order to complete this lab you will need to know the following concepts:

* Docker Images
* Docker Containers
* Docker Hub

### Docker Images

The "official" definition:

> Docker images are the basis of containers. An Image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime. An image typically contains a union of layered filesystems stacked on top of each other. An image does not have state and it never changes.

*Source: [https://docs.docker.com/engine/reference/glossary/#/image](https://docs.docker.com/engine/reference/glossary/#/image)*

Docker Images can best be compared to templates from the VM world.  They contain everything that is needed to run a container from, very important is that these images are immutable.

### Docker Containers

The "official" definition:

> A container is a runtime instance of a docker image.

*Source: [https://docs.docker.com/engine/reference/glossary/#/container](https://docs.docker.com/engine/reference/glossary/#/container)*

Docker Containers are running instances of a Docker Image, Docker Containers are mutable in the sense that they have a read-write layer on top of all the read-only layers of the Docker Image.

### Docker Hub

The "official" definition:

> The Docker Hub is a centralized resource for working with Docker and its components. It provides the following services:
> - Docker image hosting
> - User authentication
> - Automated image builds and work-flow tools such as build triggers and web hooks
> - Integration with GitHub and Bitbucket

*Source: [https://docs.docker.com/engine/reference/glossary/#/docker-hub](https://docs.docker.com/engine/reference/glossary/#/docker-hub)*

Docker Hub is to Docker Images what GitHub is to code repositories.  It has a huge collection of ready-to-use images.
