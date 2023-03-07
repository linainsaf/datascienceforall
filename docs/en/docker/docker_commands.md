# Docker CLI Commands

The Docker CLI (Command Line Interface) provides a set of commands for working with Docker images and containers. These commands are used to build, run, manage, and interact with Docker images and containers.

Here are some of the most common Docker CLI commands:

## `docker build`
This command is used to build a Docker image from a Dockerfile.
Example:
```
docker build -t myimage .
```
This command builds a Docker image from the Dockerfile in the current directory and tags it with the name `myimage`. The option `-t` or `--tag` : Sets the name and optionally a tag for the Docker image.

### docker build options

#### Tag `-t`
Example:
```
docker build -t myimage:latest .
```
or 
```
docker build -t myimage:01 .
```

This command builds a Docker image from the Dockerfile in the current directory, tags it with the name myimage and the `latest` or `01` tags.

#### File `-f`

The option `-f`, `--file` : Specifies the name and location of the Dockerfile to use.
Example:
```
docker build -t myimage:latest -f path/to/Dockerfile.dev .
```
This command builds a Docker image from the Dockerfile located at `path/to/Dockerfile.dev`, tags it with the name `myimage` and the `latest` tag.

#### Cache `--no-cache`
`--no-cache` : Disables caching during the build process.
Example:
```
docker build --no-cache -t myimage:latest .
```
This command builds a Docker image from the Dockerfile in the current directory, without using any cached layers.

## `docker run`
This command is used to run a Docker container from a Docker image.

Example:
```
docker run --name mycontainer myimage
```
This command runs a Docker container from the `myimage` Docker image and names the container `mycontainer` with the tag `--name`. 

### docker run options 

#### Tag `-d`
The option `-d`, `--detach` : Runs the container in detached mode, in the background so you can use your terminal as you want is not stuck in the process.
Example:
```
docker run -d myimage
```
This command runs the `myimage` Docker image in detached mode, in the background.

#### Tag `-p`

The option `-p`, `--publish` : Publishes a container's port(s) to the host machine.
Example:
```
docker run -p 80:80 myimage
```
This command runs the myimage Docker image and maps port `80` inside the container to port `80` on the host machine.

#### Tag `--name`

The option `--name`: Assigns a name to the container.
Example:
```
docker run --name mycontainer myimage
```
This command runs the `myimage` Docker image and assigns the name `mycontainer` to the resulting container.

#### Tag `-e`
The option  `-e`, `--env` : Sets environment variables inside the container.
Example:
```
docker run -e MYVAR=myvalue myimage
```
This command runs the `myimage` Docker image and sets the environment variable `MYVAR` to `myvalue` inside the container.

#### Tag `-v`
The option `-v`, `--volume` : Mounts a volume from the host machine into the container.
Example:
```
docker run -v /path/on/host:/path/in/container myimage
```
This command runs the `myimage` Docker image and mounts the directory `/path/on/host` on the host machine to the directory `/path/in/container` inside the container.

#### Tag `-it`

The option `-it`, `--interactive` : Runs the container in interactive mode, allowing input from the user.
Example:
```
docker run -it myimage /bin/bash
```
This command runs the `myimage` Docker image in interactive mode and starts a bash shell inside the container.

You can of course use multiple tags like : 
```
docker run -d --name mycontainer -p 80:80 myimage
```
This command runs a Docker container from the myimage Docker image in detached mode (`-d`), names the container mycontainer (`--name`), maps port `80` on the host machine to port `80` inside the container (`-p`), and uses the myimage Docker image as the container's base image.

## `docker ps`
This command is used to list running Docker containers.
Example:
```
docker ps
```
This command lists all running Docker containers. You can also list the exited container with the `-a` option , it is very usefull in case you want to debug a container.

```
docker ps -a
```

## `docker stop`
This command is used to stop a running Docker container.
Example:
```
docker stop mycontainer
```
This command stops the mycontainer Docker container.

## `docker rm`
This command is used to remove a stopped Docker container.
Example:
```
docker rm mycontainer
```
This command removes the mycontainer Docker container.

## `docker images`
This command is used to list Docker images.
Example:
```
docker images
```
This command lists all Docker images on the local machine.

## `docker rmi`
This command is used to remove a Docker image.

Example:
```
docker rmi myimage
```
This command removes the myimage Docker image.

## `docker exec`
This command is used to execute a command inside a running Docker container.

Example:
```
docker exec mycontainer ls /app
```
This command executes the ls `/app` command inside the mycontainer Docker container.

## `docker logs`
This command is used to view the logs for a Docker container.

Example:
```
docker logs mycontainer
```
This command displays the logs for the mycontainer Docker container.

## `docker inspect`
This command is used to view detailed information about a Docker object, such as a container or image.

Example:
```
docker inspect mycontainer
```
This command displays detailed information about the mycontainer Docker container.

## `docker pull`
This command is used to pull a Docker image from a registry.

Example:
```
docker pull nginx:latest
```
This command pulls the latest version of the nginx Docker image from the Docker Hub registry.

## `docker push`
This command is used to push a Docker image to a registry.
Example:
```
docker push myregistry/myimage:latest
```
This command pushes the myimage Docker image with the latest tag to the myregistry Docker registry.


## Wrap-up

These are just a few of the most common Docker CLI commands. There are many other commands available that can be used for more advanced use cases, such as networking, volumes, and swarm management. By mastering these basic Docker CLI commands, you can get started with Docker and start


