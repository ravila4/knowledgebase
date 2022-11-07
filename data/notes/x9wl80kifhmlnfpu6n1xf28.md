
## Installing Docker

Installation steps have changed quite frequently with new releases of Docker. I will do my best to keep this up-to date, but no guarantees. Check your current release's instructions.

### Ubuntu installation (July 2020)

Older versions of Docker were called docker, docker.io, or docker-engine. Now, the packages to install are docker-ce, docker-ce-cli, and containerd.io.

Official documentation:

[https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)

For Ubuntu 20.04, follow instructions [here](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04).

### Fedora installation (January 2020)

    sudo dnf install docker
    sudo sudo systemctl enable docker.service
    sudo systemctl start docker.service

**Note:** Fedora 31 had problems starting the docker daemon.

Debugging with: `sudo dockerd --debug` shows "Devices cgroup isn't mounted". The solution was to modify the grub in order to start cgroup


    sudo dnf install -y grubby
    sudo grubby --update-kernel=ALL --args="systemd.unified_cgroup_hierarchy=0"

Reboot computer.

Finally, add user to the Docker group:

    sudo groupadd docker && sudo gpasswd -a ${USER} docker

Log out and in again to see changes.


## Getting Docker Images

Pull from Docker Hub:

    docker pull hub.docker.com/r/microsoft/powershell

We may need to specify a tag, e.g.:

    docker pull ouchc/relion3.1:ubuntu16.04

Search for images: `docker search`

See installed images: `docker images`

## Running Containers

Use `docker run` to initially run a container.

### Common options:

-d
: run in background (daemonize)

-i
: interactive

-t
: tty

-p
: make a port available outside container (**HOST:CONTAINER**)

-v
: mount a volume

--name
: assign container a name

--rm
: remove container when it exits

## Interacting with containers

### Basic commands

Once a container is running, you can easily start or stop it.

Start a container (can use an assigned name): `docker start`

Stop a container `docker stop`

Execute a command off a running container: `docker exec`

Look inside container: `docker logs`

### Connect to a container as root

`docker exec -u 0 -it container_id /bin/bash`

## Listing Containers in Docker

To show only running containers use:

    docker container ls

The old, shorter way of doing this is `docker ps`.

### Options:

-a
: see all containers

-s
: see container sizes



## Clearing Non-running Containers

Containers that are not running are not taking any system resources besides disk space.

The docker run flag `--rm` will automatically remove a container when it exits.

Remove a specific container: `docker rm 3e552code34a`

Removes all stopped containers: `docker container prune`

Clean unused containers, networks, images, and volumes: `docker system prune`

## Mounting a Host Filesystem

Use the `-v` flag:

    docker run -v /host/directory:/container/directory -other -options image_name command_to_run


### Fix for permissions issues

In Fedora, SELinux can cause issues with mounting volumes.
The solution is to issue a SELinux rule:
`chcon -Rt svirt_sandbox_file_t /path/to/volume`


This got easier recently since Docker finally merged a patch in docker-1.7.

This patch adds support for "z" and "Z" as options on the volume mounts (-v).

For example:

    docker run -v /var/db:/var/db:z rhel7 /bin/sh

Will automatically do the `chcon -Rt svirt_sandbox_file_t /var/db`.

Even better, you can use Z.

    docker run -v /var/db:/var/db:Z rhel7 /bin/sh

This will label the content inside the container with the exact MCS label that the container will run with, basically it runs `chcon -Rt svirt_sandbox_file_t -l s0:c1,c2 /var/db`,
where s0:c1,c2 differs for each container.

For more info: [https://docs.docker.com/storage/volumes/](https://docs.docker.com/storage/volumes/)

**e.g.** to run my powershell docker container with a mounted home directory:

```
docker run -v /home/ravila/:/home/ravila/:Z -it mcr.microsoft.com/powershell
```

## Mapping Network Ports

    docker run --name mycontainer -p 8080:8080 -p 8000:8000

## Saving Changes to an Image

If you change anything (like install new packages) in the running container and exit the container the changes are not automatically saved. If you want to save them in an image, **use `docker commit`**.


## Docker Files

A simple example Dockerfile:

```docker
# Base image
FROM fedora:latest
RUN dnf -y update && dnf -y install unzip openssl  && dnf clean all
COPY UnblockNeteaseMusic-linux-arm7.zip /opt
RUN unzip /opt/UnblockNeteaseMusic-linux-arm7.zip -d /opt/
WORKDIR /opt/UnblockNeteaseMusic
RUN  ./createCertificate.sh
ENTRYPOINT ["/opt/UnblockNeteaseMusic/UnblockNeteaseMusic", " -b", " -e"]
EXPOSE 443
```

Docker file reference: https://docs.docker.com/engine/reference/builder/

Best practices: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/


### Build Dockerfile

In a directory with a Dockerfile run:

`sudo docker build -t "my-image" .`

If the build is successful you can see  `my-image` in docker images output.

## Docker Compose

https://docs.docker.com/compose/

https://developer.fedoraproject.org/tools/docker/compose.html

Docker Compose is a tool to orchestrate Docker containers using a simple YAML file which describes your whole setup.

Installation: `sudo dnf install docker-compose`

### Run a docker compose file

Create a YAML file: `docker-compose.yml`, then in the same directory, run:

`docker-compose up`