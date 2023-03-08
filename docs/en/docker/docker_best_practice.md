# Best Practices 

## Best Practices for design and optimize containers

### Use an appropriate base image 

The base image you choose can greatly affect the size and security of your final image. Choose a minimal base image and only include what you need to minimize the attack surface and reduce the image size.

One popular base image is `Alpine` Linux. Alpine Linux is a lightweight Linux distribution that is designed to be small and efficient. It is commonly used for Docker images because of its small size, which makes it ideal for running containers with limited resources.

Another base image that is commonly used is the `slim` version of the official images provided by different software vendors. For example, the official Python image has a slim version, which is a smaller image with only the necessary dependencies required to run Python applications. This means that you can reduce the size of your Docker image by using the slim version instead of the full version.

*However, it is important to keep in mind that using a base image that is too small can sometimes cause issues.*

### Avoid running as root 

Running containers as the root user is considered bad practice because it poses a security risk. By default, the root user inside a container has the same privileges as the root user on the host machine. This means that if an attacker gains control of a container running as root, they could potentially escalate their privileges to the host machine.

To avoid running containers as root, you can specify a non-root user in your Dockerfile using the `USER` instruction. For example:
```dockerfile
FROM python:3.9-slim

# Create a non-root user
RUN useradd --create-home myuser
WORKDIR /home/myuser

# Switch to the non-root user
USER myuser

# Copy application files and install dependencies
COPY requirements.txt .
RUN pip install -r requirements.txt

# Copy the rest of the application code
COPY . .

# Run the application
CMD ["python", "app.py"]
```

In this example, we create a non-root user called myuser and switch to that user using the USER instruction. This user is then used to run the application inside the container.

### Keep layers small 

When building a Docker image, each line in the Dockerfile creates a new layer in the final image. Layers are stacked on top of each other to create the final image. Each layer only stores the changes made on top of the previous layer, which results in a more efficient use of disk space and a faster build time.

Keeping layers small is important because it can make it easier to update or modify specific parts of the image without rebuilding the entire image. This can be especially important when dealing with large applications that have many dependencies.

![Screenshot](https://sachcode.com/static/3422749f7f459abcf3de835201c77794/c1b63/docker-layers.png)

To keep layers small, it is best to group related commands together in a single line in the Dockerfile. For example, instead of installing several packages in separate RUN commands, it is better to install them all in a single RUN command. This will result in fewer layers and a smaller image size.

It is also important to clean up any temporary files created during the build process, as these files can add unnecessary weight to the image. The `RUN` command should be followed by a `CLEANUP` command to remove any unwanted files and dependencies.

Additionally, using the `--no-cache` flag when building an image can help to reduce the size of layers, as it prevents Docker from caching layers and forces it to rebuild each layer from scratch.

### Use multi-stage builds 

Multi-stage builds are a way to optimize your Docker images and reduce their size. It allows you to use multiple FROM statements in your Dockerfile, each of which specifies a different base image.

Here's an example of how you might use multi-stage builds to build a Python application using Flask:
```dockerfile 
# Use an official Python runtime as a parent image
FROM python:3.8-slim-buster AS base

# Set the working directory to /app
WORKDIR /app

# Copy the requirements file to the working directory
COPY requirements.txt .

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Use a smaller image as the final base image
FROM python:3.8-slim-buster

# Set the working directory to /app
WORKDIR /app

# Copy the requirements file and installed packages from the previous stage
COPY --from=base /usr/local/lib/python3.8/site-packages /usr/local/lib/python3.8/site-packages

# Copy the rest of the application code to the working directory
COPY . .

# Start the Flask application
CMD ["python", "app.py"]
```
In this example, the Dockerfile uses two stages. The first stage starts with the official Python 3.8 slim-buster image, sets the working directory to `/app`, and copies the `requirements.txt` file to the working directory. It then installs the required packages specified in the `requirements.txt` file and saves them to the image.

The second stage starts with the same `Python 3.8 slim-buster` image as the final base image. It sets the working directory to `/app`, copies the installed packages from the first stage to the image, copies the rest of the application code to the working directory, and starts the Flask application.

Using multi-stage builds can significantly reduce the size of your Docker images because you only include the necessary files and dependencies in the final image. In this example, the final image only includes the installed Python packages and the application code, which makes it much smaller than if it included the entire Python runtime.

Overall, using multi-stage builds is a best practice for optimizing your Docker images and reducing their size.

### Use caching to speed up builds 

In Docker, every instruction in a Dockerfile creates a layer like we seen before. When a Dockerfile is built, Docker caches each layer, so that if the same instruction is used in a future build, Docker can use the cached layer instead of re-executing the instruction. This can greatly speed up the build process.

One way to take advantage of caching is to order the instructions in the Dockerfile such that the ones that change frequently are at the end, while the ones that change less frequently are at the beginning. For example, you might start with a base image, then copy in your application code, and finally install any dependencies.

It's also possible to explicitly tell Docker to use a cached layer with the --cache-from flag. This can be useful if you have multiple Dockerfiles that share some of the same layers. For example, if you have a base image that is used by multiple applications, you can build that image once and then use it as the cache for future builds of the applications.

Here's an example of how to use caching to speed up a Docker build:

```dockerfile
# Use an official Python runtime as a parent image
FROM python:3.9-slim-buster

# Set the working directory to /app
WORKDIR /app

# Copy the requirements file into the container
COPY requirements.txt .

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code into the container
COPY . .

# Expose port 80 for the web application
EXPOSE 80

# Start the web application
CMD ["python", "app.py"]
```

In this example, the `COPY requirements.txt .` and `RUN pip install --no-cache-dir -r requirements.txt` instructions are near the beginning of the Dockerfile because they change less frequently than the application code. This means that Docker can cache those layers and reuse them in future builds, even if the application code has changed.




### Clean up after yourself 

Cleaning up after yourself is an important best practice for Docker. This means removing any unused images, containers, volumes, and networks that are no longer needed. Not only does it save disk space, but it also ensures that the Docker environment is not cluttered with unnecessary artifacts that may cause conflicts or security issues.

Here are some tips for cleaning up after yourself in Docker:

1. **Remove unused containers**: To remove unused containers, use the `docker container prune` command. This command removes all stopped containers. If you want to remove a specific container, use the `docker rm` command followed by the container ID.
2. **Remove unused images**: To remove unused images, use the `docker image prune` command. This command removes all images that are not associated with any container. If you want to remove a specific image, use the `docker rmi` command followed by the image ID.
3. **Remove unused volumes**: To remove unused volumes, use the `docker volume prune` command. This command removes all volumes that are not associated with any container. If you want to remove a specific volume, use the `docker volume rm` command followed by the volume name.
4. **Remove unused networks**: To remove unused networks, use the `docker network prune` command. This command removes all networks that are not associated with any container. If you want to remove a specific network, use the `docker network rm` command followed by the network name.
5. **Use `--rm` option**: When running containers, use the `--rm` option to automatically remove the container when it exits. This is especially useful for testing and development environments where you don't need to keep the container around.

By following these best cleaning practices, you can keep your Docker environment clean and avoid cluttering it with unused artifacts.

### Consider security

Security is an important consideration when it comes to Docker containers. Here are some best practices to keep in mind:

1. **Use the latest version of the base image**: It is important to use the latest version of the base image, as this will ensure that any security vulnerabilities in the base image have been patched.
2. **Avoid using root user**: Running a container as root can be risky, as it can potentially allow an attacker to gain access to the host system. Instead, use a non-root user.
3. **Limit container capabilities**: By default, containers have access to all capabilities of the host system. It is important to limit the capabilities of the container to only what it needs.
4. **Use a minimal base image**: Using a minimal base image, such as Alpine, reduces the attack surface of the container by reducing the number of packages installed.
5. **Keep containers up to date**: It is important to keep containers up to date with security patches and updates.
6. **Scan images for vulnerabilities**: Use a vulnerability scanner to scan images for known vulnerabilities. This will help to identify any potential security issues.
7. **Consider network security**: Secure network access to containers by using network segmentation, firewalls, and VPNs. A common practice is to run containers inside your cloud provider's VPC. 

#### Quick scan a container 

1. First, install Trivy by following the installation instructions for your operating system from the official Trivy GitHub page: [https://github.com/aquasecurity/trivy](https://github.com/aquasecurity/trivy)

2. Then Pull a Docker image that you want to scan, for example, the official Python image:
```
docker pull python:3.9-slim
```

3. Run Trivy against the image:
```
trivy image python:3.9-slim
```
This will scan the Python image and report any vulnerabilities found in the image's base image or any installed packages.
You can also scan a Dockerfile directly using the --file option:
```
trivy --file Dockerfile
```
This will scan the Dockerfile and report any vulnerabilities found in the base image or any installed packages.

Note that scanning Docker images is just one part of a comprehensive security strategy for containerized applications. It's also important to ensure that your containers are configured securely, that you use strong authentication and authorization mechanisms, and that you regularly apply security updates and patches to your container images.

By following these best practices, you can help to ensure that your Docker containers are secure and less vulnerable to attack.

## Debugging and Troubleshooting 

1. **Check container logs**: Container logs can provide valuable information on what is happening inside the container. Use `docker logs <container-id>` to view container logs and troubleshoot issues.
2. **Use docker exec to troubleshoot running containers**: `docker exec` allows you to run commands inside a running container, which can be useful for troubleshooting issues.
3. **Check container health**: Use `docker ps` to check the status of running containers. If a container is not running, use `docker ps -a` to view all containers, including stopped ones.
4. **Check resource utilization**: Docker containers can consume a lot of resources. Use `docker stats` to view resource utilization for running containers.
5. **Use the correct Docker command**: Use the correct Docker command for the task at hand. For example, `docker stop` will gracefully stop a container, while `docker kill` will forcibly stop a container.
6. **Check networking**: If your container is not communicating with other containers or services, check the networking configuration. Use docker network ls to view available networks and `docker network inspect` to view details of a specific network.
7. **Consider container security**: Ensure that your container is running in a secure environment and follow security best practices for your application like we have seen before. 

By following best practices for Dockerfile design and container optimization and knowing how to troubleshoot common issues with Docker containers, you can ensure that your Docker containers are running smoothly and securely.