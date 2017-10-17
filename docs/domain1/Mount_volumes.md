# Mount volumes

### Give a service access to volumes or bind mounts

For best performance and portability, you should avoid writing important data
directly into a container's writable layer, instead using data volumes or bind
mounts. This principle also applies to services.

You can create two types of mounts for services in a swarm, `volume` mounts or
`bind` mounts. Regardless of which type of mount you use, configure it using the
`--mount` flag when you create a service, or the `--mount-add` or `--mount-rm`
flag when updating an existing service.. The default is a data volume if you
don't specify a type.

#### Data volumes

Data volumes are storage that remain alive after a container for a task has
been removed. The preferred method to mount volumes is to leverage an existing
volume:

```bash
$ docker service create \
  --mount src=<VOLUME-NAME>,dst=<CONTAINER-PATH> \
  --name myservice \
  <IMAGE>
```

For more information on how to create a volume, see the `volume create`
[CLI reference](/engine/reference/commandline/volume_create.md).

The following method creates the volume at deployment time when the scheduler
dispatches a task, just before starting the container:

```bash
$ docker service create \
  --mount type=volume,src=<VOLUME-NAME>,dst=<CONTAINER-PATH>,volume-driver=<DRIVER>,volume-opt=<KEY0>=<VALUE0>,volume-opt=<KEY1>=<VALUE1>
  --name myservice \
  <IMAGE>
```

> **Important:** If your volume driver accepts a comma-separated list as an option,
> you must escape the value from the outer CSV parser. To escape a `volume-opt`,
> surround it with double quotes (`"`) and surround the entire mount parameter
> with single quotes (`'`).
>
> For example, the `local` driver accepts mount options as a comma-separated
> list in the `o` parameter. This example shows the correct way to escape the list.
>
>     $ docker service create \
>          --mount 'type=volume,src=<VOLUME-NAME>,dst=<CONTAINER-PATH>,volume-driver=local,volume-opt=type=nfs,volume-opt=device=<nfs-server>:<nfs-path>,"volume-opt=o=addr=<nfs-address>,vers=4,soft,timeo=180,bg,tcp,rw"'
>         --name myservice \
>         <IMAGE>


#### Bind mounts

Bind mounts are file system paths from the host where the scheduler deploys
the container for the task. Docker mounts the path into the container. The
file system path must exist before the swarm initializes the container for the
task.

The following examples show bind mount syntax:

- To mount a read-write bind:

  ```bash
  $ docker service create \
    --mount type=bind,src=<HOST-PATH>,dst=<CONTAINER-PATH> \
    --name myservice \
    <IMAGE>
  ```

- To mount a read-only bind:

  ```bash
  $ docker service create \
    --mount type=bind,src=<HOST-PATH>,dst=<CONTAINER-PATH>,readonly \
    --name myservice \
    <IMAGE>
  ```

> **Important**: Bind mounts can be useful but they can also cause problems. In
> most cases, it is recommended that you architect your application such that
> mounting paths from the host is unnecessary. The main risks include the
> following:
>
> - If you bind mount a host path into your service’s containers, the path
>   must exist on every swarm node. The Docker swarm mode scheduler can schedule
>   containers on any machine that meets resource availability requirements
>   and satisfies all constraints and placement preferences you specify.
>
> - The Docker swarm mode scheduler may reschedule your running service
>   containers at any time if they become unhealthy or unreachable.
>
> - Host bind mounts are completely non-portable. When you use bind mounts,
>   there is no guarantee that your application will run the same way in
>   development as it does in production.

### Create services using templates

You can use templates for some flags of `service create`, using the syntax
provided by the Go's [text/template](http://golang.org/pkg/text/template/)
package.

The following flags are supported:

- `--hostname`
- `--mount`
- `--env`

Valid placeholders for the Go template are:

| Placeholder       | Description    |
|:------------------|:---------------|
| `.Service.ID`     | Service ID     |
| `.Service.Name`   | Service name   |
| `.Service.Labels` | Service labels |
| `.Node.ID`        | Node ID        |
| `.Task.Name`      | Task name      |
| `.Task.Slot`      | Task slot      |

#### Template example

This example sets the template of the created containers based on the
service's name and the ID of the node where the container is running:

```bash
{% raw %}
$ docker service create --name hosttempl \
                        --hostname="{{.Node.ID}}-{{.Service.Name}}"\
                         busybox top
{% endraw %}
```

To see the result of using the template, use the `docker service ps` and
`docker inspect` commands.

```bash
$ docker service ps va8ew30grofhjoychbr6iot8c

ID            NAME         IMAGE                                                                                   NODE          DESIRED STATE  CURRENT STATE               ERROR  PORTS
wo41w8hg8qan  hosttempl.1  busybox:latest@sha256:29f5d56d12684887bdfa50dcd29fc31eea4aaf4ad3bec43daf19026a7ce69912  2e7a8a9c4da2  Running        Running about a minute ago
```

```bash
{% raw %}
$ docker inspect --format="{{.Config.Hostname}}" hosttempl.1.wo41w8hg8qanxwjwsg4kxpprj
{% endraw %}
```
