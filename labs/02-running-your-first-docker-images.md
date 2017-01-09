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

## Running one-off commands

As you probably noticed, none the above commands (`docker run centos` and `docker run ubuntu`) produced any output.  The reason is that we did not specify which command we wanted to run inside the container.  By design containers are designed to run a single process and terminated as soon as that process is finished.  Since we did not specify a process (command), our container is terminated immedetiatly after it was started.

To tell Docker to run a specific command, simply add it behind the `docker run <IMAGE_NAME>` command.  Below we will be outputting the hostname of the container:

```
docker run centos hostname 
```

As you can tell this hostname is different from the hostname of the Linux host:

```
root@ip-172-31-26-41:~# hostname
ip-172-31-26-41

root@ip-172-31-26-41:~# docker run centos hostname
ef5d46ccb19f
```

## Working "inside" the container

If you want to get "inside" a container you can run the command below, it will start a container from the image you specified and provide you with shell access (bash) to the container:

```
docker run -ti centos bash
```

NOTE: the `-t` option is to provide a pseudo tty and the `-i` is to make stdin interactive, basically you will need these 2 options to be able to interact with your container

Once you have issued the above command you will notice that your command prompt has changed, you are now inside your container.  Every command you issue at this prompt will be executed inside the container.  To get back to prompt of your host (Ubuntu) simply issue the `exit` command on the prompt of the container.

```
[root@d10fa581a6bf /]# exit
exit
root@ip-172-31-26-41:~#
```

So now you should be able to run `yum update` inside your CentOS container on an Ubuntu host.

```
root@ip-172-31-26-41:~# yum update
The program 'yum' is currently not installed. You can install it by typing:
apt install yum

root@ip-172-31-26-41:~# docker run -ti centos bash

[root@ea8c7d186b49 /]# yum -y update
Loaded plugins: fastestmirror, ovl
base                                                                                                                                                                                 | 3.6 kB  00:00:00
extras                                                                                                                                                                               | 3.4 kB  00:00:00
updates                                                                                                                                                                              | 3.4 kB  00:00:00
(1/4): extras/7/x86_64/primary_db                                                                                                                                                    | 183 kB  00:00:00
(2/4): base/7/x86_64/group_gz                                                                                                                                                        | 155 kB  00:00:00
(3/4): updates/7/x86_64/primary_db                                                                                                                                                   | 1.2 MB  00:00:00
(4/4): base/7/x86_64/primary_db                                                                                                                                                      | 5.6 MB  00:00:00
Determining fastest mirrors
 * base: ftp.heanet.ie
 * extras: ftp.heanet.ie
 * updates: ftp.heanet.ie
Resolving Dependencies
--> Running transaction check
---> Package vim-minimal.x86_64 2:7.4.160-1.el7 will be updated
---> Package vim-minimal.x86_64 2:7.4.160-1.el7_3.1 will be an update
--> Finished Dependency Resolution
<<<...>>>

[root@ea8c7d186b49 /]# exit
```
