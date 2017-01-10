# Lab 03 - Running more advanced Docker containers

## Prerequisites

Before proceeding with this lab, please familiarize yourself first with the topics below:

* [Docker Hub](https://docs.docker.com/engine/reference/glossary/#/docker-hub)

## Running containers as deamons (services)

So far we only ran short-lived containers (one-off commands & interactive shells).  Docker containers are of course most often used for long running processes (services), such as web and/or application servers.  In order to run a container in deamon mode, we need to start an image that launches a long-runnin process and add `-d` option to our `docker run <IMAGE_NAME>` command.

Let us start an nginx container in deamon mode:

```
docker run -d nginx
```

When we check the `docker ps` output we indeed see that the nginx container is still running:

```
root@ip-172-31-26-41:~# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
a9c2cf7e91ab        nginx               "nginx -g 'daemon off"   36 seconds ago      Up 36 seconds       80/tcp, 443/tcp     backstabbing_ramanujan
```

In the output above we also see the ports that nginx is running on, 80 and 443.  As nginx is running inside the container we need to find out the IP of the container that nginx is running in, this can be done using the following command:

```
docker inspect --format '{{ .NetworkSettings.IPAddress }}' a9c2cf7e91ab
```

Using the IP returned by the above command we can access nginx running inside our container.

```
curl 172.17.0.2
curl nodeXY.<PROJECT_NAME>.gluo.io		# this command will not work
```

## Exposing container ports on the host

In the above example `172.17.0.2` is of course an internal IP address.  So our nginx service is only accessible from our Ubuntu host.  If we want our nginx service running inside our container to be accessible from outside (the Internet) we will need to add another option to our `docker run -d <IMAGE_NAME>` command, more specifically the `-p` option.

The command below will start an nginx container and will expose port 80 from our nginx container to port 80 on our Ubuntu host:

```
docker run -d -p 80:80 nginx
```

When we run `docker ps` again, we will see a second container, this time we see that it is also mapped to port 80 on our Ubuntu host (0.0.0.0:80->80/tcp):

```
root@ip-172-31-26-41:~# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
8243b6b6af62        nginx               "nginx -g 'daemon off"   2 seconds ago       Up 2 seconds        0.0.0.0:80->80/tcp, 443/tcp   fervent_chandrasekhar
a9c2cf7e91ab        nginx               "nginx -g 'daemon off"   13 minutes ago      Up 13 minutes       80/tcp, 443/tcp               backstabbing_ramanujan
```

We can now test if our nginx service is indeed accessible from the Internet:

```
curl nodeXY.<PROJECT_NAME>.gluo.io
```

## Getting "inside" running Docker containers

In the previous lab we have seen that we can get inside a new container using `docker run -ti <IMAGE_NAME> bash`, to get inside a running container we will need the `docker exec` command, below is an example to get inside the 8243b6b6af62 container (the container ID can be found using the `docker ps` command).

```
docker exec -ti 8243b6b6af62 bash
```

When ready with your work inside the container simply type `exit` to exit the container, when you run `docker ps` you will see that your container will still be running.

## Stopping and re-starting containers

To stop a running container, the `docker stop <CONTAINER_ID>` command can be used, for example:

```
docker stop 8243b6b6af62
```

To start it again use the `docker start <CONTAINER_ID>` command, keep in mind that stopped container will only show up when you use the `-a` option with the `docker ps` command:

```
root@ip-172-31-26-41:~# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
8243b6b6af62        nginx               "nginx -g 'daemon off"   14 minutes ago      Up 13 seconds       0.0.0.0:80->80/tcp, 443/tcp   fervent_chandrasekhar
a9c2cf7e91ab        nginx               "nginx -g 'daemon off"   28 minutes ago      Up 28 minutes       80/tcp, 443/tcp               backstabbing_ramanujan

root@ip-172-31-26-41:~# docker stop 8243b6b6af62
8243b6b6af62

root@ip-172-31-26-41:~# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
a9c2cf7e91ab        nginx               "nginx -g 'daemon off"   28 minutes ago      Up 28 minutes       80/tcp, 443/tcp     backstabbing_ramanujan

root@ip-172-31-26-41:~# docker start 8243b6b6af62
8243b6b6af62

root@ip-172-31-26-41:~# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
8243b6b6af62        nginx               "nginx -g 'daemon off"   14 minutes ago      Up 2 seconds        0.0.0.0:80->80/tcp, 443/tcp   fervent_chandrasekhar
a9c2cf7e91ab        nginx               "nginx -g 'daemon off"   28 minutes ago      Up 28 minutes       80/tcp, 443/tcp               backstabbing_ramanujan
```

## Removing containers and images

If you no longer require a container and/or image you can remove them with the following commands.

Removing a Docker container (keep in mind that the container has to be stopped first `docker stop <CONTAINER_ID>`)

```
docker ps

# get container ID

docker rm 8243b6b6af62
```

Removing a Docker images (keep in mind that all containers that were started from that image have to be removed first).

```
docker images

# get image ID

docker rmi 01f818af747d
``` 

## Additional usefull commands

If you want to remove all stopped containers you can use the command below (use with care of course):

```
docker rm $(docker ps -qa)
```

If you want to remove all the images you can use the command below:

```
docker rmi $(docker images -qa)
```
