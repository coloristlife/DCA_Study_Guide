## Domain 2: Image Creation, Management, and Registry

**Describe Dockerfile options**
> search options

**Show the main parts of a Dockerfile**
* `FROM`: Initializes a new build stage and sets the Base Image 
* `RUN`: Execute any commands in a new layer on top of the current image 
* `CMD`: Defaults for an executing container
    * Sould be an executable
    * Only 1 per file
    * Should be in JSON format
* `LABEL`: Adds metadata to an image
* `EXPOSE`: Listens on the specified network ports at runtime
* `ENV`: Sets the environment variable `<key>` to the value `<value>`
* `ADD`: Copies new files, directories or remote file URLs from `<src>` to container's `<dest>`
* `COPY`: Copies new files or directories from `<src>` and adds them to the filesystem of the container at the path `<dest>`
* `ENTRYPOINT`: Allows you to configure a container that will run as an executable
* `VOLUME`: Creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers
* `WORKDIR`: Sets the working directory for instructions that follow it in the Dockerfile

.
* Give examples on how to create an efficient image via a Dockerfile
* Use CLI commands such as list delete prune rmi etc to manage images
* Inspect images and report specific attributes using filter and format
* Demonstrate tagging an image
* Utilize a registry to store an image
* Display layers of a Docker image
* Apply a file to create a Docker image
* Modify an image to a single layer
* Describe how image layers work
* Deploy a registry
* Configure a registry
* Log into a registry
* Utilize search in a registry
* Tag an image
* Push an image to a registry
* Sign an image in a registry
* Pull an image from a registry
* Describe how image deletion works
* Delete an image from a registry
