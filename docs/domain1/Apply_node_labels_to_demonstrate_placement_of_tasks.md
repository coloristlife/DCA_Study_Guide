# Apply node labels to demonstrate placement of tasks

## Official Docker Documentation
[Create Labels on Docker Swarm Nodes](https://docs.docker.com/engine/reference/commandline/node_update/)  
[Specifiy Services Placement Preferences](https://docs.docker.com/engine/swarm/services/#specify-service-placement-preferences-placement-pref)

## Preparation Hints
There exists the possibilty to add custom labels on nearly every Docker Object: [Manage labels on objects](https://docs.docker.com/engine/userguide/labels-custom-metadata/#manage-labels-on-objects)


Add label metadata to a node
Add metadata to a swarm node using node labels. You can specify a node label as a key with an empty value:

$ docker node update --label-add foo worker1
To add multiple labels to a node, pass the --label-add flag for each label:

$ docker node update --label-add foo --label-add bar worker1
When you create a service, you can use node labels as a constraint. A constraint limits the nodes where the scheduler deploys tasks for a service.

For example, to add a type label to identify nodes where the scheduler should deploy message queue service tasks:

$ docker node update --label-add type=queue worker1
The labels you set for nodes using docker node update apply only to the node entity within the swarm. Do not confuse them with the docker daemon labels for dockerd.

For more information about labels, refer to apply custom metadata.
