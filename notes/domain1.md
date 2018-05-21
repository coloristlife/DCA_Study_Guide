## Domain 1: Orchestration

**Complete the setup of a swarm mode cluster with managers and worker nodes**
* Run on the master: `docker swarm init --advertise-addr <MANAGER-IP>`
* Verify swarm mode by: `docker info`
* Verify node status by: `docker node ls`

**State the differences between running a container vs running a service**
* Running container is Docker in *single engine mode*
* Running a service is when Docker is running in *swarm mode*

**Demonstrate steps to lock a swarm cluster**
* To initialize a swarm from rebooting master nodes: `docker swarm init --autolock`
* To enable autolock: `docker swarm update --autolock=true`
* To disable autolock: `docker swarm update --autolock=false`
* To unlock a swarm: `docker swarm unlock`
* View current key: `docker swarm unlock-key`
* Rotate current key: `docker swarm unlock-key --rotate`

**Extend the instructions to run individual containers into running services under swarm**
* To create a new service: `docker service create --replicas 1 --name helloworld alpine ping docker.com`
    * The `docker service create` command creates the service.
    * The `--name` flag names the service `helloworld`.
    * The `--replicas` flag specifies the desired state of 1 running instance.
    * The arguments `alpine ping docker.com` define the service as an Alpine
    Linux container that executes the command `ping docker.com`.
* To view current services running: `docker service ls`

**Interpret the output of docker inspect commands**
* To get a list of services: `docker service ps`
* To inspect a service: `docker service inspect <SERVICE-ID>`
* Consider viewing the yaml output: `docker service inspect --pretty <SERVICE-ID>`

**Convert an application deployment into a stack file using a YAML compose file with docker stack deploy**
* Command usage: `docker stack deploy [OPTIONS] STACK`
* Example: `docker stack deploy --compose-file docker-compose.yml vossibility`
> TODO: Read more about Docker Stack

**Manipulate a running stack of services**
* To list docker stack services: `docker stack ls`
* Available Stack commands: deploy, ls(stacks), ps(tasks), rm, services
> TODO: Read more about Docker Stack

**Increase number of replicas**
* To update existing service: `docker service update --replicas=NUMBER SERVICE`
* To update existing service: `docker service scale SERVICE=NUMBER`
* To update existing service: `docker service scale SERVICE1=NUMBER SERVICE2=NUMBER`
* To view number of replicas: `docker service ls`

**Add networks publish ports**
* Create a service: `docker service create nginx && docker service ls`
* `docker service create --name helloworld alpine ping docker.com`
* Update the service: `docker service update --publish-add 80 nginx`
* To publish a service's ports externally: `docker service create --publish 8080:80 nginx`
* Usage: `--publish <TARGET-PORT>:<SERVICE-PORT>`

**Mount volumes**
```bash
$ docker service create \
  --mount src=<VOLUME-NAME>,dst=<CONTAINER-PATH> \
  --name myservice \
  <IMAGE>
```

**Illustrate running a replicated vs global service**
* To inspect current mode: `docker service inspect SERVICE --pretty | grep "Service Mode"`
* A replicated service is replicated to the replicas number
* A global service runs on all available nodes

**Identify the steps needed to troubleshoot a service not deploying**
* `docker service ls`
* `docker service ps SERVICE`
* `docker service insect SERVICE`
* `docker logs CONTAINER`
* To prevent a service from being deployed, scale the service to 0

**Apply node labels to demonstrate placement of tasks**
* Add metadata to a swarm node using node labels
* `docker node update --label-add foo worker1`
* When you create a service, you can use node labels as a constraint. A constraint limits the nodes where the scheduler deploys tasks for a service

**Sketch how a Dockerized application communicates with legacy systems**
* donno...

**Paraphrase the importance of quorum in a swarm cluster**
* Always have more than two managers
* Odd number of managers is recommended
* To add new nodes: `docker swarm join-token (worker|manager)`

**Demonstrate the usage of templates with docker service create**
* Templated configs can be used in: --hostname, --mount, --env
* Example: `docker service create --name hosttempl \
            --hostname="{{.Node.Hostname}}-{{.Node.ID}}-{{.Service.Name}}" busybox top`
