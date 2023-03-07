# Building a Docker Image for your Python App

Docker provides a convenient way to package your Python applications and dependencies into a self-contained image that can be run on any machine. In this tutorial, we will walk you through the process of building a Docker image for a standard Python app.

## Common problems if you choose NOT to use Docker

Here are three common problems you might encounter if you choose not to use Docker for your Python script deployment:

- **Dependency Conflicts**: One of the biggest challenges with Python application deployment is managing dependencies. Without Docker, it can be difficult to ensure that your Python application and its dependencies will work correctly on different machines and environments. This can result in dependency conflicts, broken code, and lost productivity.
- **Inconsistency**: Another issue with deploying Python applications without Docker is inconsistency. Different machines and environments can have different versions of Python, different system libraries, and different configurations. This can make it difficult to reproduce and debug issues, and can result in code that works on some machines but not on others.
- **Limited Portability**: Without Docker, it can be difficult to move your Python application between different machines and environments. This can limit your ability to scale and deploy your application effectively, and can result in lost opportunities and increased costs.

Overall, while it is possible to deploy Python applications without Docker, doing so can lead to dependency conflicts, inconsistency, and limited portability. **By using Docker to package your Python application and its dependencies into a self-contained container, you can ensure that your application runs consistently and reliably across different machines and environments.**


## Why we use Docker to run Python scripts in containers:

- **Isolation**: Running a Python script in a Docker container provides a degree of isolation between the script and the host machine, which helps to minimize conflicts and dependencies with other applications or processes.
- **Consistency**: By running a Python script in a Docker container, you can ensure that the environment in which the script runs is consistent across different machines and environments. This helps to avoid the "it works on my machine" problem and makes it easier to reproduce and debug issues.
- **Portability**: Docker containers are self-contained units that can be easily moved between different machines and environments. This makes it easy to deploy and scale Python scripts in a variety of settings, from local development to production servers.
- **Efficiency**: Docker containers are lightweight and efficient, which means that they can be deployed quickly and consume minimal resources. This makes them an ideal choice for running Python scripts that need to be deployed quickly or scaled up or down rapidly.

## Creating a Dockerfile for our python app

The first step in building a Docker image for your Python app is to create a Dockerfile. The Dockerfile is a text file that contains a set of instructions for building the Docker image. Here's an example Dockerfile for a Python app:
```Dockerfile 
# Use an official Python runtime as a parent image
FROM python:3.9

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

### Let's see how this file works line by line


#### Defining the base image and environment

The first line in the Dockerfile specifies the base image to use for the container. In this case, we are using the official `Python 3.9` image.
```Dockerfile 
FROM python:3.9
```

Next, we set the working directory to "/app" inside the container.
```Dockerfile 
WORKDIR /app
```
This is the directory where we will copy our application code and dependencies.

#### Installing dependencies and copying source code
Next, we copy our application code and dependencies into the container. This is done using the `COPY` command.
```Dockerfile
COPY . /app
```
This command copies the contents of the current directory into the "/app" directory inside the container.

We then install any dependencies that are needed for our application. This is done using the "RUN" command.
```Dockerfile
RUN pip install --no-cache-dir -r requirements.txt
```
This command reads the requirements.txt file and installs any dependencies listed in the file.

#### Configuring the app and exposing ports
Next, we configure our application by setting any environment variables and exposing any necessary ports.
```Dockerfile
EXPOSE 80
ENV NAME World
```
In this example, we are exposing port 80 and setting an environment variable named "NAME" to "World".

Finally, we specify the command to run when the container is launched.
```Dockerfile
CMD ["python", "app.py"]
```

This command specifies that the "app.py" file should be executed when the container is launched.

### Building the Docker image
Now that we have created our Dockerfile, we can build our Docker image. 
To do this, we use the `docker build` command like this : 
```bash 
docker build -t my-python-app .
```

This command tells Docker to build an image with the name `my-python-app` using the Dockerfile in the current directory (.) 

### Running the Docker container
Once we have built our Docker image, we can run it using the `docker run` command : 
```bash 
docker run -p 4000:80 my-python-app
```
This command tells Docker to run the `my-python-app` image and map port `4000` on the host machine to port `80` inside the container.

## Summary of the most common Dockerfile commands

These commands can be combined in various ways to create a Dockerfile for your specific application. By using Dockerfile commands, you can define the steps needed to build a Docker image and run a Docker container for your application.

Most common Dockerfile commands: 

- `FROM`: Specifies the base image for the Docker image.
- `RUN`: Executes a command during the build process, such as installing dependencies or running tests.
- `COPY` or `ADD`: Copies files or directories from the host machine into the Docker image.
- `WORKDIR`: Sets the working directory inside the Docker image.
- `EXPOSE`: Exposes a port for the Docker container.
- `ENV`: Sets an environment variable inside the Docker image.
- `CMD` or `ENTRYPOINT`: Specifies the command to run when the Docker container starts.

<center>
![Screenshot](https://miro.medium.com/v2/resize:fit:426/1*E_id80g9vERM-n3TnVdimw.jpeg)
</center>

These commands can be combined in various ways to create a Dockerfile for your specific application. By using Dockerfile commands, you can define the steps needed to build a Docker image and run a Docker container for your application.

## Run a FastAPI "Hello World" Python app into a container

**Create a new Python file named `main.py`** with the following code : 
```python title="main.py"
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World!"}
```

**Create a new file named `Dockerfile`** in the same directory with the following contents:
```Dockerfile 
FROM tiangolo/uvicorn-gunicorn-fastapi:python3.8-slim

COPY ./app /app

EXPOSE 80

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "80"]
```

This Dockerfile uses the `tiangolo/uvicorn-gunicorn-fastapi` base image, copies the app directory (which contains main.py) into the container, exposes port `80`, and sets the `CMD` to run the uvicorn server with the `main:app` parameter.

**Build the Docker image** using the following command:
```bash 
docker build -t fastapi-demo .
```
This command builds the Docker image and tags it with the name fastapi-demo.

**Run the Docker container** using the following command:
```bash
docker run -p 80:80 fastapi-demo
```
This command starts the Docker container and maps port 80 on the host machine to port 80 inside the container.

**Make sure the container is up** and the API is running with the command : 
```bash 
docker ps 
```

If you see the container UP and running, then you can open your web browser and navigate to [http://localhost:80/](http://localhost:80/). You should see the message "Hello World!" displayed in your browser 🥳

### Why `0.0.0.0` and not `localhost`

When setting up a Docker container, it's common to bind the container's internal port to a port on the host machine so that the container's services can be accessed from the outside. When specifying the IP address for the --host parameter in the uvicorn command, you have a choice between using `localhost` and `0.0.0.0`

Using localhost as the IP address for the --host parameter means that the server will only accept requests coming from within the container itself. This can be useful if you want to restrict access to the server to only the container itself.

However, if you want to allow external access to the server (i.e., from the host machine or other machines on the same network), you should use `0.0.0.0` as the IP address for the --host parameter. This tells the server to accept requests from any IP address.

So, **in a FastAPI application context for a Docker container, using `0.0.0.0` as the IP address for the --host parameter allows the container's services to be accessed from the host machine or other machines on the same network, while using localhost would restrict access to only the container itself**.
## Wrap-up 

In summary, building a Docker image for your Python app involves creating a Dockerfile, defining the base image and environment, installing dependencies and copying source code, configuring the app and exposing ports, and finally building and running the Docker image. 




