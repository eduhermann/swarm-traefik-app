# swarm-traefik-app

### Requirements

* Docker installed on three nodes with the Swarm Cluster active. Prefer using the three nodes as master.
* HAProxy installed and configured for load balancer between nodes Swarm Cluster.

### Topology

Simple topology based in one cluster Docker Swarm, composed for three nodes, two nodes to HAProxy load balancer (install and configure by HAProxy won't be part of this repository).

![Topology Swarm Traefik App](https://github.com/eduhermann/swarm-traefik-app/blob/main/resources/diagram.png)

### Resources Used

Here are some resources used to perform the environment deployment:

Remember: you can adaptive the environment, if necessary.

* Main resources:
    * 05 VMs
        * S.O. Ubuntu 20.04 LTS
        * 02 GB RAM
        * 02 core CPU
        * 01 network card

* VMs:
    * 3 VMs nodes Swarm 
    * 2 VMs HAProxy

It's possible to remove "High Availability Zone" layer (HAProxy zone), but will be necessary to configure DNS resolve to each IP node Docker Swarm.

### Running the Application

1 - Clone the repository:
```
$ git clone https://github.com/eduhermann/swarm-traefik-app.git
```

2 - Change the necessary variables according to your environment:
```
File: deploy.yml
    - "traefik.http.routers.webserver.rule=Host(`YOU_URL_HERE`)"
```

2 - Build the application image:
```
$ cd ./app-build/

$ docker image build -t web-app:1.0 .
```

* The image must be sent to a registry or else it must be built on all nodes of the cluster.

3 - Creating a new network:
```
$ docker network create -d overlay traefik-public
```

4 - Create the new stack in the Swarm cluster:

```
$ docker stack deploy -c deploy.yml [NOME_STACK]
```

5 - Tests:
* Access the URL defined on step 2, at port 80, ensure that the IP or name will be accessible from machine that will do tests.
* Access the URL defined on step 2, at port 8080 to visualize the status from Traefik.

### Enjoy!