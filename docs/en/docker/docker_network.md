# What is Docker Networking?

Docker Networking allows you to connect Docker containers together so that they can communicate with each other. This is useful for building complex applications that are made up of multiple containers, each with its own functionality.

<br />

Docker Networking also allows you to isolate containers from each other, providing an added layer of security. Additionally, Docker Networking makes it easy to connect containers to external networks, such as the internet, and to other Docker hosts.

## Docker Network Types

Docker supports several types of network drivers that provide different ways to connect containers together. Here are some of the most common Docker network types:

- **Bridge Network**: The default network type in Docker, a bridge network is a private network that allows containers to communicate with each other using IP addresses. Containers on a bridge network can communicate with each other but are isolated from the host machine and external networks.
- **Host Network**: A host network allows containers to use the host machine's network stack, essentially giving them direct access to the host's network interfaces. This can provide better performance but may not be as secure as other network types.
- **Overlay Network**: An overlay network allows you to connect containers that are running on different Docker hosts. This is useful for building distributed applications that are made up of multiple Docker hosts.
- **Macvlan Network**: A macvlan network allows you to assign a MAC address to a container, essentially making it appear as though it is a physical machine on the network. This can be useful for running containers that require direct access to the physical network.

## Creating a Docker Network

Creating a Docker network is easy. You can use the docker network create command to create a new network:
```bash 
docker network create mynetwork
```
This command creates a new Docker network with the name mynetwork.

## Attaching Containers to a Network

To attach a container to a network, you can use the --network option when you start the container:
```bash 
docker run --name mycontainer --network mynetwork alpine sleep 3000
```

This command creates a new container with the name mycontainer and attaches it to the mynetwork network.

## Connecting to External Networks

To connect a container to an external network, such as the internet, you can use the --network option to specify the host network:
```bash 
docker run --name mycontainer --network host alpine ping google.com
```
This command creates a new container with the name mycontainer and attaches it to the host network. The container then uses the host machine's network stack to ping google.com.

## Create containers and attach them to a network

### Step 1: Create a Docker Network

The first step is to create a Docker network that both containers will be attached to. This can be done using the docker network create command:
```bash 
docker network create mynetwork
```

### Step 2: Create the First Container

Next, we'll create the first container and attach it to the mynetwork network. We'll use the docker run command to create the container:
```bash
docker run --name container1 --network mynetwork alpine sleep 3000
```

This command creates a new container with the name container1, attaches it to the mynetwork network, and starts the sleep command to keep the container running for 3000 seconds.

### Step 3: Create the Second Container

Next, we'll create the second container and attach it to the mynetwork network. We'll use the docker run command again:
```bash
docker run --name container2 --network mynetwork alpine sleep 3000
```
Same thing, this command creates a new container with the name `container2`, attaches it to the mynetwork network, and starts the sleep command to keep the container running for 3000 seconds.

### Step 4: Create the Third Container

Now, let's create a third container that is not attached to the mynetwork network. We'll use the docker run command to create the container:
```bash
docker run --name container3 alpine sleep 3000
```
This command creates a new container with the name `container3` and starts the sleep command to keep the container running for 3000 seconds. Since we did not specify a network for this container, it will be attached to the default bridge network.

### Step 5: Ping One Container from the Other

#### What is `ping`

The `ping` command is commonly used to test the availability and responsiveness of network devices, such as servers or routers. It can help diagnose network connectivity issues, such as packet loss or latency.

<br />

When you run the `ping` command, it will send packets of data to the specified destination, and display the results in the terminal. The output will typically include statistics about the packet transmission, such as the number of packets sent and received, the round-trip time (RTT) for each packet, and any errors or packet loss that occurred during the transmission.

<br />

Here's an example of running the ping command:
```
ping google.com
```

This command sends packets of data to the Google.com domain name, and displays the results in the terminal. The output will show the RTT for each packet, as well as other statistics about the packet transmission.

#### Ping  `container1` from `container2`

Now that both containers are running and attached to the same network, we can confirm that they can communicate with each other. We'll do this by pinging `container1` from `container2`:
```bash 
docker exec container2 ping container1
```
This command uses the docker exec command to run the ping container1 command inside container2. If the two containers are able to communicate with each other, you should see output similar to the following:
```bash 
PING container1 (172.19.0.2): 56 data bytes
64 bytes from 172.19.0.2: seq=0 ttl=64 time=0.091 ms
64 bytes from 172.19.0.2: seq=1 ttl=64 time=0.111 ms
``` 
If you see this output, it means that the two containers are able to communicate with each other over the `mynetwork` network.

<br />

Now, let's try to ping `container1` from `container3`, which is not attached to the `mynetwork` network:

```bash 
docker exec container3 ping container1
```
This command uses the docker exec command to run the ping container1 command inside `container3`. Since `container3` is not attached to the mynetwork network, it should not be able to communicate with container1. You should see output similar to the following:

```bash 
ping: bad address 'container1'
```

This output confirms that `container3` is not able to communicate with `container1`.

## Wrap-up 

Docker Networking is a powerful feature that allows you to connect Docker containers together so that they can communicate with each other. By mastering Docker Networking, you can build complex applications that are made up of multiple containers, each with its own functionality. You can also isolate containers from each other, connect them to external networks, and build distributed applications that are made up of multiple Docker hosts.