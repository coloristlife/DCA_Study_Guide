# Manipulate a running stack of services

A running stack of services on Docker Swarm can be manipulated in different ways: `docker stack ls` shows an overview of all existing stacks. `docker stack ps STACK` lists all tasks in a stack, while `docker stack services STACK` gives an overview over the running services. `docker stack rm STACK` removes said stack.

# stack

```markdown
Usage:  docker stack COMMAND

Manage Docker stacks

Options:
      --help   Print usage

Commands:
  deploy      Deploy a new stack or update an existing stack
  ls          List stacks
  ps          List the tasks in the stack
  rm          Remove the stack
  services    List the services in the stack

Run 'docker stack COMMAND --help' for more information on a command.
```

## Description

Manage stacks.

