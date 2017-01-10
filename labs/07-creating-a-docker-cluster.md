# Lab 07 - Creating a Docker cluster

## Prerequisites

Before proceeding with this lab, please familiarize yourself first with the topics below:

* [Docker swarm](https://docs.docker.com/engine/reference/glossary/#/swarm)

**NOTE: we will have 2 different clusters, an "odd" one and an "even one**

## Initializing a Docker Swarm cluster

The following command should only be run on `node01` and `node02`:

```
docker swarm init --advertise-addr $(hostname -i) 
```

The output of the above command should be copied somewhere safe, the other teams will need this output (a command) to have their node join the cluster:

```
    docker swarm join \
    --token SWMTKN-1-587xm9zr5tiraj0s7tfpyys60r7fszn6itxhnxhsfucelg833x-eimr2hkkhy30vxrcpzh5t0ala \
    172.31.5.47:2377
```

NOTE: if you do happen to lose the command, you can always get it back by running `docker swarm join-token worker` command

## Joining an existing Docker Swarm cluster

All odd nodes (node03, node05,...) need to join the cluster initialized by node01.  All even nodes (node04, node06,...) need to join the cluster initialized by node02.

To have your node join the cluster simple run the command that is provided to you by the team running node01/node02:

```
    docker swarm join \
    --token SWMTKN-1-587xm9zr5tiraj0s7tfpyys60r7fszn6itxhnxhsfucelg833x-eimr2hkkhy30vxrcpzh5t0ala \
    172.31.5.47:2377
```

## Verifying the cluster

To ensure that the cluster is running as it should you can run the `docker info` and `docker node ls` commands.

```
docker info

<... ...>
Swarm: active
 NodeID: ezh08kvrya4rtfywc3ph0fyzi
 Is Manager: true
 ClusterID: 8gpcrfgexsisag5piy42jfhqw
 Managers: 1
 Nodes: 1
 Orchestration:
  Task History Retention Limit: 5
<... ...> 

docker node ls
ID                           HOSTNAME         STATUS  AVAILABILITY  MANAGER STATUS
5izxh6aatcwgxn4kmy7wtgbbu    ip-172-31-3-241  Ready   Active
ezh08kvrya4rtfywc3ph0fyzi *  ip-172-31-5-47   Ready   Active        Leader
```
## Start the Docker Swarm visualizer

The Swarm visualizer only has to be run on `node01` and `node02`:

```
docker run -it -d -p 8000:8080 -v /var/run/docker.sock:/var/run/docker.sock manomarks/visualizer
```

You can visit the Docker Swarm visualizer on:
- http://node01.PROJECT_NAME.gluo.io:9000
- http://node02.PROJECT_NAME.gluo.io:9000

## Start a service on the Docker Swarm cluster

This should also only be done on `node01` and `node02`.

We will now be deploying our container-info container onto the cluster:

```
docker service create --publish 80:80 --replicas 1 --name container-info trescst/container-info
```

You can now surf to any node on the cluster and you will be able to see the container-info "website".  When you check the visualizer you will see that a container was started on one of the nodes of the cluster.

## Scaling a service

Now that we got everything up-and-running, the fun begins!  You can easily scale a service using the `docker service scale` command.  After you have scaled your service make sure to check the visualizer.

```
docker service scale container-info=2
docker service scale container-info=5
docker service scale container-info=1
docker service scale container-info-10
```
