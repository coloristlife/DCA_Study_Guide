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

* Convert an application deployment into a stack file using a YAML compose file with docker stack deploy
* Manipulate a running stack of services
* Increase number of replicas
* Add networks publish ports
* Mount volumes
* Illustrate running a replicated vs global service
* Identify the steps needed to troubleshoot a service not deploying
* Apply node labels to demonstrate placement of tasks
* Sketch how a Dockerized application communicates with legacy systems
* Paraphrase the importance of quorum in a swarm cluster
* Demonstrate the usage of templates with docker service create
