# Cloud registery

A Docker Registry is a centralized service for storing and distributing Docker images. It enables you to share your images with others, collaborate on projects, and deploy containerized applications more easily.

Why is Docker Registry practical for developers?

1. Streamlines collaboration: Developers can share Docker images with teammates or the public, making it easy to collaborate on projects.
Version control: Docker Registry stores different versions of images, allowing you to maintain a history of your application and roll back to previous versions if needed.
- Simplified deployment: Docker images can be easily deployed to various environments (e.g., development, staging, production) by pulling them from the registry.
- Integration: Docker Registry integrates with CI/CD pipelines, allowing for automated building, testing, and deployment of containerized applications.
- Consistency: By using a Docker Registry, you ensure that everyone on your team is using the same image, reducing inconsistencies and potential errors.

## Push a simple container to Docker Hub

### Create a Dockerfile 
In an empty directory, create a file named `Dockerfile` with the following content:
```dockerfile
FROM nginx:alpine
COPY ./index.html /usr/share/nginx/html/
```
This Dockerfile uses the nginx:alpine base image and copies an index.html file into the container.
Now, create index.html file in the same directory with the following content:
```html 
<!DOCTYPE html>
<html>
<head>
  <title>Simple Docker Container</title>
</head>
<body>
  <h1>Hello from a simple Docker container!</h1>
</body>
</html>
```

### Build the Docker image 
Open a terminal, navigate to the directory containing the Dockerfile and index.html, and run the following command:
```bash
docker build -t [DOCKER_HUB_USERNAME]/simple-container:latest .
```
Replace `[DOCKER_HUB_USERNAME]` with your Docker Hub username. This command builds the Docker image and tags it with your Docker Hub username.

### Create a Docker Hub account 
If you don't have a Docker Hub account, sign up for one at [https://hub.docker.com/signup](https://hub.docker.com/signup)
Log in to Docker Hub: Run the following command to log in to Docker Hub from the terminal:
```
docker login
```
Enter your Docker Hub username and password when prompted.

### Push the Docker image to Docker Hub 
Run the following command to push the image to Docker Hub:
```bash
docker push [DOCKER_HUB_USERNAME]/simple-container:latest
```
Replace `[DOCKER_HUB_USERNAME]` with your Docker Hub username. The command uploads the image to your Docker Hub account.

After completing these steps, you will have successfully pushed a simple container to Docker Hub. You can now view and manage your image on the Docker Hub website by navigating to your repositories.

## Why cloud registery 
Pushing containers to a cloud-based container registry offers numerous benefits, making it a good practice for managing and deploying containerized applications. Here are some reasons why:

- **Centralized storage**: A cloud-based container registry provides a centralized location to store and manage all your container images. This makes it easier to organize, version, and track changes across multiple images used by different teams or projects within your organization.
- **Accessibility**: Storing container images in a cloud registry ensures they are readily available for deployment across different environments, such as development, staging, and production. This simplifies the process of deploying and scaling your application across various cloud resources and regions.
- **Security**: Cloud-based container registries often come with built-in security features, such as vulnerability scanning, private repositories, and access controls. This allows you to identify potential security risks in your images and ensure that only authorized users can access or modify them.
- **Integration**: Cloud container registries are designed to integrate seamlessly with other cloud services, such as container orchestration platforms (e.g., Kubernetes) and CI/CD pipelines. This makes it easier to automate deployments, scaling, and updates for your containerized applications.
- **Performance**: Cloud container registries typically offer fast, reliable, and geographically distributed storage. This ensures that your container images can be quickly and efficiently pulled and deployed to your cloud resources, reducing latency and improving the overall performance of your application.
- **Collaboration**: Using a cloud-based container registry makes it easier for teams to collaborate on containerized applications. Developers can share images, track changes, and work together to maintain and improve the application, regardless of their physical location.
- Versioning and rollback: Storing images in a container registry allows you to maintain different versions of your application. This makes it easier to roll back to a previous version in case of issues or to deploy different versions of the application for testing purposes.


In summary, using a cloud-based container registry is a good practice because it centralizes storage, enhances accessibility and security, improves collaboration, and simplifies the deployment and management of containerized applications.

## Push images on Google Cloud

First, install and configure the Google Cloud SDK: If you haven't already, install the Google Cloud SDK on your local machine. You can find installation instructions for various platforms here: [https://cloud.google.com/sdk/docs/install](https://cloud.google.com/sdk/docs/install)

Authenticate with Google Cloud: Run the following command to authenticate your Google Cloud account:
```
gcloud auth login
```
This command will open a web page where you can enter your Google Cloud credentials. After successfully logging in, the terminal will display a confirmation message.

### Set up

1. Configure Docker to use gcloud as a credential helper: Run the following command to configure Docker to use the gcloud command-line tool as a credential helper:
```
gcloud auth configure-docker
```
This command will add an entry to your Docker configuration file `~/.docker/config.json`, enabling the Docker client to use the Google Cloud SDK for authentication.

2. Verify IAM permissions: Ensure that your Google Cloud account has the necessary permissions to push images to the Container Registry. You need the "Storage Admin" role (or a custom role with equivalent permissions) for the project.
To check your permissions, follow these steps:
    - Go to the Google Cloud Console: [https://console.cloud.google.com/](https://console.cloud.google.com/)
    - Click on the project dropdown and select your project.
    - On the left-hand menu, click `"IAM & Admin" > "IAM"`.
    - Find your account in the list, and check if it has the `Storage Admin` role. If not, click the pencil icon next to your account to edit the permissions, add the `Storage Admin` role, and save the changes.

3. Re-authenticate
Run the following command to re-authenticate your Google Cloud account : 
```
gcloud auth login
```
This command will open a web page where you can enter your Google Cloud credentials. After successfully logging in, the terminal will display a confirmation message.

4. Configure the project
Set the default project for the gcloud command-line tool by running:
```
gcloud config set project [PROJECT_ID]
```
Replace `[PROJECT_ID]` with your Google Cloud project ID.

### Tag the local Docker image

To push your local Docker image to GCR, you first need to tag it using the GCR repository format. The format is as follows:
```bash
gcr.io/[PROJECT_ID]/[IMAGE_NAME]:[TAG]
```
Run the following command to tag your local image:
```bash
docker tag [LOCAL_IMAGE_NAME]:[TAG] gcr.io/[PROJECT_ID]/[IMAGE_NAME]:[TAG]
```
Replace `[LOCAL_IMAGE_NAME]`, `[TAG]`, `[PROJECT_ID]`, and `[IMAGE_NAME]` with the appropriate values.

### Enable the Google Container Registry API 

Before you can push images to GCR, ensure that the Google Container Registry API is enabled for your project. Navigate to the following URL and replace `[PROJECT_ID]` with your project ID:
```ruby
https://console.cloud.google.com/apis/library/containerregistry.googleapis.com?project=[PROJECT_ID]
```
Click the `Enable` button if the API is not already enabled.

### Push the image to GCR 
Finally, run the following command to push your tagged Docker image to GCR:
```bash 
docker push gcr.io/[PROJECT_ID]/[IMAGE_NAME]:[TAG]
```
Your local Docker image should now be uploaded to your Google Cloud Container Registry. You can verify this by checking the "Container Registry" section in the Google Cloud Console.

You can con inside you browser then google cloud console to find it or just pull it with this command : 
```bash 
docker pull gcr.io/[PROJECT_ID]/[IMAGE_NAME]:[TAG]
```
