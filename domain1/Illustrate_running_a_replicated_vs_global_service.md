# Illustrate running a replicated vs global service


A **service** is the definition of the tasks to execute on the manager or worker nodes. It
is the central structure of the swarm system and the primary root of user
interaction with the swarm.

When you create a service, you specify which container image to use and which
commands to execute inside running containers.

In the **replicated services** model, the swarm manager distributes a specific
number of replica tasks among the nodes based upon the scale you set in the
desired state.

For **global services**, the swarm runs one task for the service on every
available node in the cluster.

A **task** carries a Docker container and the commands to run inside the
container. It is the atomic scheduling unit of swarm. Manager nodes assign tasks
to worker nodes according to the number of replicas set in the service scale.
Once a task is assigned to a node, it cannot move to another node. It can only
run on the assigned node or fail.
