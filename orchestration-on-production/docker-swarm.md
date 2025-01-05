A swarm is a set of nodes which are either physical or virtual servers. Each swarm must have at
least one manager node which manages the service: a set of tasks (containers) is distributed to
worker nodes.

command:

- docker system info
  shows -> Swarm: inactive

Execute the following command to define your machine as the swarm manager:

- docker swarm init

The result:
Swarm initialized: current node (iez9608gohisunu7cysg6x5l2) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-63rs917cvprqquipzq5tcqwhbl2d9ymcl4t5uoaot4vexg97lk-0b7ozxbzlcmhm0hpp5w6w2yvv 192.168.65.3:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

Other physical or virtual devices on your network which have Docker installed can join the swarm
by issuing the (long) docker swarm join --token command shown.

Enter - docker node ls
to view all nodes within your swarm. A single development machine
will display something similar to:
ID HOSTNAME STATUS AVAILABILITY MANAGER STATUS ENGINE VERSION
iez9608gohisunu7cysg6x5l2 \* docker-desktop Ready Active Leader 27.4.0

Pull any images you require before creating a service - unlike
docker run and docker compose, they won't be pulled automatically, .e.g.

- docker pull jazperydio0224/nodehello

Then create a service which launches a single container on the swarm:

- docker service create --name nodehello -p 3000:3000 jazperydio0224/nodehello

Execute - docker service ls
to show running tasks:
ID NAME MODE REPLICAS IMAGE PORTS
ln0nttyzfrh4 nodehello replicated 1/1 jazperydio0224/nodehello:latest \*:3000->3000/tcp

**Scale a swarm service**
Your application can be scaled as it becomes increasingly popular. To launch three container
instance for the nodehello service, enter:

- docker service scale nodehello=3

Alternatively, the

- docker service create
  command has an optional --replicas arg which could have been used
  instead of scaling later, e.g.
- docker service create --name nodehello -p 3000:3000 --replicas=3 jazperydio0224/nodehello

**Stop and remove a swam service**
Services can be listed by executing docker service ls.
A service name can then be passed to the rm command to stop it, e.g.

- docker service rm nodehello

All containers started on swarm nodes are automaticall stopped and removed.

**Remove a node from a swarm**
Any worker node can leave the swam by entering this command on the device:

- docker swarm leave

The manager node can leave the swarm and disabled swarm mode using:

- docker swarm leave --force
