# Setting up Docker

The process of installing Docker on your local machine will depend on the operating system you are using. Docker provides installation packages for Windows, macOS, and Linux.

## Installing Docker on MacOS

1. Go to the official [docker website](https://www.docker.com) and download the appropriate installation package for your operating system.
*Be aware of your chipset for example if your Apple machine is new install the version apple chip not intel (you can see that chip information if you go on the `little  on the top left of your screen` > `about my Mac`)*

2. Follow the installation instructions 
3. Once Docker is successfully installed, open a terminal or command prompt and run the following command to verify that Docker is running:
```bash
docker version
```
If Docker is running correctly, you should see information about the Docker version, API version, and other details like this : 
```bash 
Client:
 Cloud integration: 1.0.14
 Version:           20.10.6
 API version:       1.41
 Go version:        go1.16.3
 Git commit:        370c289
 Built:             Fri Apr  9 22:46:57 2021
 OS/Arch:           darwin/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.6
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       8728dd2
  Built:            Fri Apr  9 22:44:56 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.4
  GitCommit:        05f951a3781f4f2c1911b05e61c160e9c30eaa8e
 runc:
  Version:          1.0.0-rc93
  GitCommit:        12644e614e25b05da6fd08a38ffa0cfe1903fdec
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
``` 

### Technical informations about docker version 

Just in case if you wondering what the output means 🤓

- **Client**: This section shows the version and details of the Docker client that is running on your local machine. The client is responsible for issuing commands to the Docker daemon, which manages containers and images.
    - Cloud integration: This shows the version of the Docker Cloud integration plugin that is installed on your machine. Docker Cloud is a service that provides tools for managing Docker containers in the cloud.
    - **Version**: This shows the version of the Docker client that is installed on your machine.
    - **API version**: This shows the version of the Docker API that is supported by the client.
    - Go version: This shows the version of the Go programming language that was used to compile the Docker client.
    - Git commit: This shows the Git commit hash that was used to build the Docker client.
    - Built: This shows the date and time that the Docker client was built.
    - OS/Arch: This shows the operating system and processor architecture that the Docker client is running on.
    - Context: This shows the default Docker context that is currently in use.
    - Experimental: This shows whether experimental features are enabled on the Docker client.
- **Server**: This section shows the version and details of the Docker daemon that is running on your local machine. The daemon is responsible for managing containers and images.
    - **Engine**: This shows the version of the Docker engine that is running on your machine.
    - **API version**: This shows the version of the Docker API that is supported by the engine.
    - Go version: This shows the version of the Go programming language that was used to compile the Docker engine.
    - Git commit: This shows the Git commit hash that was used to build the Docker engine.
    - Built: This shows the date and time that the Docker engine was built.
    - **OS/Arch**: This shows the operating system and processor architecture that the Docker engine is running on.
    - Experimental: This shows whether experimental features are enabled on the Docker engine.
    - containerd: This shows the version and Git commit of containerd, which is the container runtime used by Docker.
    - runc: This shows the version and Git commit of runc, which is the command-line tool used to run containers.
    - docker-init: This shows the version and Git commit of docker-init, which is the initialization script used by Docker to start containers.


## Docker client and Docker server 

As you can see the output of the `docker version` command is divided into two sections: Client and Server.
In the context of Docker, the Docker client and server are two separate components that work together to manage containers and images.

### Docker client 

The **Docker client is a command-line interface (CLI) tool that allows you to interact with the Docker daemon**. The client sends commands to the daemon, which then executes those commands and manages the containers and images on your system. The Docker client can be used to build and run containers, manage images, and perform other Docker-related tasks.

### Docker server 

The **Docker server, also known as the Docker daemon, is a background process that manages the containers and images on your system**. The daemon listens for commands from the Docker client, executes those commands, and manages the underlying infrastructure needed to run containers, such as networking and storage.

The Docker client and server communicate with each other using the Docker API, which is a RESTful API that provides a standardized way to interact with Docker. When you run a command using the Docker client, such as "docker run", the client sends a request to the Docker daemon over the Docker API. The daemon then processes the request and executes the command.

**In summary, the Docker client is a tool for interacting with the Docker daemon, while the Docker daemon is a background process that manages the containers and images on your system. The client and daemon communicate with each other using the Docker API.**

