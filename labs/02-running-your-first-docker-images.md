# Lab 02 - Running your first Docker images

## Prerequisites 

Before proceeding with this lab, please familiarize yourself with the topics below:

* [Docker Images](https://docs.docker.com/engine/reference/glossary/#/image)
* [Docker Containers](https://docs.docker.com/engine/reference/glossary/#/container)

## Running your first container 

To run your first Docker container, simply run the command below:

```
docker run hello-world
```

When succesful you will be prompted with the following output:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker Hub account:
 https://hub.docker.com

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/
```

As you can read in the output, issuing the `docker run <IMAGE_NAME>` command does in fact 2 important things:

1. make a local copy (download) the specified image, <IMAGE_NAME>
2. start a container using the local copy of the image

## Prefetching Docker images

Because the hello-world image is so small (under 2MB uncompressed) you probably did not even notice that issuing the `docker run <IMAGE_NAME>` first triggered a download of the image.  However when we start containers from larger images we clearly see the dowload process in action.

```
docker run centos
```

You should see something similar to:
```
Unable to find image 'centos:latest' locally
latest: Pulling from library/centos

45a2e645736c: Downloading [======================================>            ] 54.61 MB/70.39 MB
```

Using the `docker pull <IMAGE_NAME>` command you can prefetch a docker image before starting containers from the image, this way your image is cached locally and the start command only takes seconds.

```
docker pull ubuntu
docker run ubuntu
```
