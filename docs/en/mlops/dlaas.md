# MLOps for Deep Learning as a Service

In this section, we will explore MLOps specifically for Deep Learning as a Service (DLaaS), focusing on the unique challenges and requirements of deploying and managing deep learning models in production.

Deep Learning as a Service (DLaaS) is a type of cloud computing service that provides access to deep learning algorithms and models, allowing data scientists and developers to build and train their own deep learning models without having to manage the underlying hardware and infrastructure.

With DLaaS, users can **rent computing power from a cloud provider to run deep learning models on large datasets, and then access the trained models from anywhere with an internet connection.** This can be a cost-effective and convenient way to use deep learning technology, particularly for smaller companies or individuals who may not have the resources to build and maintain their own infrastructure.

Some popular examples of DLaaS providers include the clouds providers like Amazon Web Services (AWS) SageMaker, Google Cloud Platform (GCP) AI Platform, and Microsoft Azure Machine Learning (other players are now in the game like Alibaba, IBM, Oracle, Hugging Face, Replit, Scaleway ...). These services typically offer pre-built machine learning models, as well as tools for building and training custom models.

In summary, Deep Learning as a Service provides a convenient and accessible way for data scientists and developers to leverage the power of deep learning algorithms and models, without having to worry about the underlying infrastructure.

## What to expect from DLaaS platform 🤔

Let's dive into some examples of what do DLaaS platforms : 

- **Data Management**: As more companies move their data from on-premise storage to cloud storage systems, the need to properly organize these data arises. Since MLaaS platforms are essentially cloud providers, that is, they offer cloud storage, they provide ways to properly manage data for machine learning experiments, data pipelining, thus making it easier for data engineers to access and process the data. 
- **Access to ML Tools**: MLaaS providers offer tools such as predictive analytics and data visualization for businesses. They also make available APIs for sentiment analysis, face recognition, creditworthiness assessments, business intelligence, healthcare, etc. 
Data scientists don’t need to worry about the actual computations of these operations because they are abstracted by MLaaS providers. Some MLaaS providers even give you a drag and drop interface for machine learning experimentation and model building (with its limitations, of course).
- **Ease of use**: MLaaS offers Data scientists the means to get started quickly with machine learning without having to endure the tedious software installation processes or provide their own servers as they would with most other cloud computing services. With MLaaS the provider’s data centers handle the actual computation, so it’s convenient at every turn for businesses.
- **Cost efficiency**: Building an ML workstation is expensive, at the time of writing this article, a single Nvidia GPU costs $699 while a Google cloud TPU v2 goes for $4.50. 
So in fact, when choosing the in-cloud TPU the data scientist would have already computed over 155 hours of experiments when reaching the initial cost of buying the Nvidia GPU. Also, chipset needs a significant amount of power to work so the electricity bill will rise 💸 MLaaS can also be beneficial in the development phase because you only pay for hardware when it is actually used.

MLaaS platforms offer these solutions and many more. Let’s have a brief overview of some platforms offering these MLaaS solutions and how they can be accessed, you can read more about it in this excellent article of [neptune.ai](https://neptune.ai/blog/machine-learning-as-a-service-what-it-is-when-to-use-it-and-what-are-the-best-tools-out-there) 🤓

I also recommand you this lecture about a benchmarck of this differents providers : [https://neptune.ai/blog/best-machine-learning-as-a-service-platforms-mlaas](https://neptune.ai/blog/best-machine-learning-as-a-service-platforms-mlaas)


## Model Versioning and Experiment Tracking 🔬

First things thirst : experimentation !
Deep learning models often involve numerous experiments with different architectures, hyperparameters, and training data. To manage this complexity, it's essential to track experiments and maintain versions of your models.

### Example: Using MLflow for model versioning and experiment tracking

Install MLflow:

```bash
pip install mlflow
```
Initialize an MLflow tracking server (replace <path_to_mlruns> with a directory path where you want to store your experiment metadata):
```bash
mlflow server \
    --backend-store-uri sqlite:///<path_to_mlruns>/mlflow.db \
    --default-artifact-root <path_to_mlruns> \
    --host 0.0.0.0
```
In your Python code, log your experiments and models with MLflow:
```python
import mlflow

# Set tracking URI to connect to the MLflow server
mlflow.set_tracking_uri("http://0.0.0.0:5000")

# Start a new experiment
mlflow.start_run(experiment_id=1)

# Log parameters, metrics, and artifacts
mlflow.log_param("learning_rate", 0.001)
mlflow.log_metric("accuracy", 0.95)
mlflow.log_artifact("model.h5")

# End the experiment
mlflow.end_run()
```

You can learn more about MLFlow by reading their tutorials : 


## Automated Model Retraining 🚃

As new data becomes available, it's crucial to retrain your deep learning models to maintain their performance. Automating this process can help ensure your models stay up-to-date.

### Example: Using Apache Airflow to automate model retraining

Install Apache Airflow:
```
pip install apache-airflow
```
Initialize the Airflow database and start the Airflow web server and scheduler:
```bash
airflow db init
airflow webserver --port 8080
airflow scheduler
```
Create a new DAG (Directed Acyclic Graph) in the Airflow dags directory:
```python title="retrain_model_dag.py"
from datetime import datetime, timedelta
from airflow import DAG
from airflow.operators.python_operator import PythonOperator

def retrain_model():
    # Add your model retraining code here

dag = DAG(
    "retrain_model",
    default_args={
        "owner": "airflow",
        "retries": 0,
        "retry_delay": timedelta(minutes=5),
    },
    description="Retrain deep learning model",
    schedule_interval=timedelta(days=1),
    start_date=datetime(2023, 1, 1),
    catchup=False,
)

retrain_model_task = PythonOperator(
    task_id="retrain_model",
    python_callable=retrain_model,
    dag=dag,
)
```
Replace the `retrain_model` function with your model retraining code.

## Scaling Model Deployments ⚖️ 

Deep learning models can be computationally intensive, and it's essential to scale your deployments to handle varying loads and maintain low latency.

### Example: Deploying a TensorFlow model using Kubernetes and TensorFlow Serving

**Prerequisites**
Before we begin, make sure you have the following:
- A trained deep learning model (e.g., TensorFlow model)
- Docker installed on your local machine
- A Google Cloud Platform (GCP) account
- Kubernetes Engine API enabled on GCP
- Google Cloud SDK (gcloud) and kubectl installed on your local machine
 
First save your TensorFlow model in the SavedModel format:

```python
model.save('saved_model')
```
This will create a saved_model directory containing the necessary files for serving your model.

Create a Dockerfile in your project directory:
```bash
FROM tensorflow/serving

WORKDIR /models/my_model
COPY ./saved_model/ .

ENV MODEL_NAME=my_model
```
Replace my_model with the name of your model.

Now let's build the Docker image with the following command:
```bash
docker build -t gcr.io/your-gcp-project-id/my-model:v1 .
```
Replace your-gcp-project-id with your actual GCP project ID.

Authenticate Docker with your GCP credentials:
```
gcloud auth configure-docker
```
Push the Docker image to Google Container Registry:
```perl
docker push gcr.io/your-gcp-project-id/my-model:v1
```
Create a Kubernetes cluster on Google Kubernetes Engine (GKE) using the following command:
```bash
gcloud container clusters create my-cluster --num-nodes=3 --zone=us-central1-a --machine-type=n1-standard-4
```
This will create a cluster with 3 nodes, each having 4 vCPUs.

Create a `deployment.yaml` file in your project directory:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-model-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-model
  template:
    metadata:
      labels:
        app: my-model
    spec:
      containers:
      - name: tensorflow-serving
        image: gcr.io/your-gcp-project-id/my-model:v1
        ports:
        - containerPort: 8501
          protocol: TCP

```
Apply the deployment file to create the model replicas:

```
kubectl apply -f deployment.yaml
```

Expose the deployment using a Kubernetes service:
```
kubectl expose deployment my-model-deployment --type=LoadBalancer --port=8501 --target-port=8501
```
This will create a load balancer that distributes incoming traffic to the model replicas.

Retrieve the external IP address of the load balancer:
```
kubectl get services
```
This will display the IP address under the `EXTERNAL-IP` column.

Now, you can send requests to your deployed model using the external IP address of the load balancer:
```
curl -X POST -d '{"instances": [input_data]}' -H "Content-Type: application/json" http://<EXTERNAL-IP>:8501/v1/models/my_model:predict
```

Make sure to replace the following placeholders with the appropriate values:

- `<EXTERNAL-IP>` : Replace with the external IP address of the load balancer that you obtained earlier.
- `input_data` : Replace with the input data for your model in the required format.

If you need to handle more requests, you can scale the number of replicas by updating the replicas field in the deployment.yaml file:
```yaml
spec:
  replicas: <NEW_NUMBER_OF_REPLICAS>
```
Replace `<NEW_NUMBER_OF_REPLICAS>` with the desired number of replicas. Then, apply the updated deployment file:
```bash
kubectl apply -f deployment.yaml
```

To monitor the performance of your deployment, you can use Kubernetes built-in tools and GKE dashboards. For example, you can use kubectl to view the status of your deployment:
```bash
kubectl get deployments
```
You can also view logs for a specific pod:
```bash
kubectl logs <POD_NAME>
```
Replace `<POD_NAME>` with the name of the pod you want to view logs for.

Once you're done testing your deployment, don't forget to delete your resources to avoid incurring unnecessary costs. To delete your Kubernetes cluster, run the following command:
```bash
gcloud container clusters delete my-cluster --zone=us-central1-a
```
To delete the Docker image from the Google Container Registry, use the following command:
```bash
gcloud container images delete gcr.io/your-gcp-project-id/my-model:v1
```
Replace `your-gcp-project-id` with your actual GCP project ID.

Now you know how to scale deep learning model deployment using Kubernetes and TensorFlow Serving. We covered creating a Docker image, deploying it to Google Kubernetes Engine, and exposing it using a load balancer. By following this mini example, you should now have a solid foundation for scaling your deep learning model deployments and handling increased traffic.

## Leverage the power of the cloud ☁️

### Hugging face 

Hugging Face is a company that provides an open-source platform for building, training, and deploying natural language processing (NLP) models. The Hugging Face platform is built on top of the PyTorch and TensorFlow frameworks, and provides a wide range of pre-trained models and tools for NLP tasks such as text classification, question answering, and language translation.

One of the **key features of the Hugging Face platform is its inference endpoint, which allows users to easily deploy and use pre-trained models for inference**. The inference endpoint provides a simple and scalable API for making predictions on new data using pre-trained models.

The first step in using the Hugging Face inference endpoint is to choose a pre-trained model that is appropriate for your NLP task. Hugging Face provides a wide range of pre-trained models for various NLP tasks, which you can browse and download from the Hugging Face model hub.

Once you've chosen a pre-trained model, you need to host it on the Hugging Face platform. To do this, you can upload the model files to the Hugging Face model hub using the Hugging Face command-line interface (CLI).

Here's an example of how to upload a pre-trained model to the Hugging Face model hub:
```bash
transformers-cli login
transformers-cli upload my-model
```

This command logs in to the Hugging Face CLI, and then uploads the model files to the Hugging Face model hub under the name "my-model".

Once you've hosted the pre-trained model on Hugging Face, you can create an inference API using the Hugging Face inference endpoint. To do this, you need to create a configuration file that specifies the model and any additional parameters that are required for inference.

Here's an example of how to create an inference API using the Hugging Face inference endpoint:

```yaml
model:
  name: my-model
  tokenizer: my-tokenizer
  framework: pytorch
  version: 1.0.0
```
This YAML file specifies the name of the pre-trained model, the name of the tokenizer to use, the framework used to train the model (in this case, PyTorch), and the version of the model.

Once you've created the configuration file, you can deploy the inference API using the Hugging Face inference endpoint. To do this, you can use the Hugging Face CLI to create a new deployment on the Hugging Face platform.

Here's an example of how to deploy the inference API using the Hugging Face CLI:

```bash
huggingface-cli inference api create my-api --model my-model.yaml
```
This command creates a new deployment on the Hugging Face platform with the name "my-api", using the configuration file "my-model.yaml".

Once the inference API has been deployed, you can use it to make predictions on new data using the Hugging Face API. To do this, you can send HTTP requests to the API endpoint that was created during deployment.
Here's an example of how to use the Hugging Face API to make predictions on new text data:

```python
import requests
import json
import transformers

# Load a sample text input
input_text = "This is a test sentence."

# Encode the input text using the tokenizer
tokenizer = transformers.AutoTokenizer.from_pretrained("my-tokenizer")
input_ids = tokenizer.encode(input_text, return_tensors="pt")

# Convert the input IDs to a JSON string
data = json.dumps({"inputs": input_ids.tolist()})

# Send an HTTP POST request to the API endpoint
headers = {"content-type": "application/json"}
response = requests.post("https://api-inference.huggingface.co/models/my-api", data=data, headers=headers)

# Parse the response and extract the predicted label
result = json.loads(response.text)
predicted_label = result["outputs"][0]
print("Predicted label:", predicted_label)
```

This example loads a sample text input, encodes it using the pre-trained tokenizer, sends an HTTP POST request to the API endpoint of the deployed model, and parses the response to extract the predicted label.

To learn more about Hugging Face API go check their documentation : [https://huggingface.co/inference-api](https://huggingface.co/inference-api)


In summary, the Hugging Face platform provides a powerful and flexible platform for building, training, and deploying NLP models. By using the Hugging Face inference endpoint and API, you can easily deploy pre-trained models for inference, and make predictions on new data using a simple and scalable API. With Hugging Face, you can accelerate your NLP development and get to production faster.

### Example: Using Replicate for 'on the fly' deployment

Let's try to build a model from scratch and deploy it on (https://replicate.com)[https://replicate.com]

In this mini tutorial, we will walk you through the process of running a simple deep learning model based on the MNIST dataset using Replicate. Replicate is a version control system for machine learning that tracks and stores model weights, training data, and other related files. This comprehensive guide will cover the necessary steps, from setting up your environment to training and evaluating your model.

**Prerequisites**
To follow this tutorial, you will need the following:

- Python 3.6 or higher
- A working installation of Replicate
- TensorFlow 2.0 or higher
- The MNIST dataset
- Install Replicate:

First, ensure that you have Replicate installed on your system. You can install it via pip:
```
pip install replicate
```
Create a new directory for your project and navigate to it:
```bash
mkdir mnist_replicate
cd mnist_replicate
```
Download the MNIST dataset using TensorFlow's built-in utility:
```python
import tensorflow as tf

mnist = tf.keras.datasets.mnist
(x_train, y_train), (x_test, y_test) = mnist.load_data()
```
Normalize the pixel values and convert the labels to one-hot encoded vectors:
```python

x_train, x_test = x_train / 255.0, x_test / 255.0

y_train = tf.keras.utils.to_categorical(y_train, 10)
y_test = tf.keras.utils.to_categorical(y_test, 10)
```
Create a very simple deep learning model using TensorFlow:
```python
def create_model():
    model = tf.keras.models.Sequential([
        tf.keras.layers.Flatten(input_shape=(28, 28)),
        tf.keras.layers.Dense(128, activation='relu'),
        tf.keras.layers.Dropout(0.2),
        tf.keras.layers.Dense(10, activation='softmax')
    ])

    model.compile(optimizer='adam',
                  loss='categorical_crossentropy',
                  metrics=['accuracy'])

    return model

model = create_model()
```
Train the model using the preprocessed data:
```python
history = model.fit(x_train, y_train, epochs=10, validation_split=0.2)
```
After training the model, save it for later use:
```python
model.save('mnist_model.h5')
```
Initialize Replicate in your project directory:
```
replicate init
```
This creates a .replicate directory and a replicate.yaml configuration file.

Now you can edit the `replicate.yaml` file and add the following lines:
```yaml
repository: /path/to/your/replicate/storage
experiments:
- name: mnist-model
  path: .
  pythonpath: .
  script: train.py
```
Replace /path/to/your/replicate/storage with the path where you want to store your Replicate data.

Create a checkpoint after training the model:
```python
import replicate

experiment = replicate.init(path=".", params={"epochs": 10})
checkpoint = experiment.checkpoint(path="mnist_model.h5", metrics={"accuracy": history.history["accuracy"][-1]})
```

Load the model from the checkpoint and evaluate it on the test data:
```python
loaded_model = tf.keras.models.load_model(checkpoint.path)
test_loss, test_accuracy = loaded_model.evaluate(x_test, y_test)
print("Test accuracy: {:.2f}".format(test_accuracy))
```

If you want to compare different experiments, you can run the same model with different hyperparameters or architecture changes. Update the training script (e.g., train.py) with new hyperparameters or changes and re-run the script.
To compare different experiments, use the Replicate Python API or the command-line interface:
```python
import replicate

experiments = replicate.list_experiments()
for exp in experiments:
    print("Experiment ID:", exp.id)
    print("Parameters:", exp.params)
    print("Best Checkpoint Accuracy:", max([ckpt.metrics["accuracy"] for ckpt in exp.checkpoints]))
```
Or, using the command-line interface:
```bash
replicate ls
```

Replicate automatically tracks changes in your code, hyperparameters, and data. You can view the differences between experiments and see which code changes led to improvements in your model's performance.
To view the differences between two experiments, use the Replicate Python API or the command-line interface:

```python
import replicate

exp1 = replicate.get_experiment("experiment_id_1")
exp2 = replicate.get_experiment("experiment_id_2")
print(replicate.compare_experiments(exp1, exp2))
```
Or, using the command-line interface:
```
replicate diff experiment_id_1 experiment_id_2
```

Replicate makes it easy to collaborate with your team. Share the repository path with your teammates, and they can clone the repository to start working on the project:
```bash
replicate clone /path/to/your/replicate/storage
```
Your teammates can now run the same experiments, compare results, and make improvements to the model.

In conclusion, we demonstrated how to run a simple deep learning model based on the MNIST dataset using Replicate. We covered the necessary steps, from setting up the environment to training and evaluating the model. We also showed how to use Replicate for version control and collaboration. By following this mini tutorial, you should now have a solid foundation for using Replicate in your deep learning projects. 

You can also deploy your model with docker, even get a GPU machine with a pay as you go option, you can check all this in their well documented doc : [https://replicate.com/docs](https://replicate.com/docs)


### Example : Weights and Biases for precise experimentation 

Weights and Biases is a cuting edge tool for visualizing and tracking the performance of machine learning models, it's used by OpenAI to train gpt3 by the way 🤓. It provides a simple and intuitive interface for logging and monitoring key metrics during model training and evaluation.

Here's a mini tutorial on how to experiment with wandb using Python.

First, you need to install the wandb library and create a wandb account:

```bash
pip install wandb
```

Then, you need to initialize wandb in your Python script:
```python
import wandb

wandb.init(project='my-project')
```

Once you've initialized wandb, you can start logging metrics and visualizations during your model training loop. Here's an example of how to log the loss and accuracy during training:

```python
for epoch in range(num_epochs):
    # Train the model
    loss, accuracy = train(model, train_data)
    
    # Log the loss and accuracy to wandb
    wandb.log({'train_loss': loss, 'train_accuracy': accuracy})
```

You can also log visualizations, such as images or plots, during training:
```python
for epoch in range(num_epochs):
    # Train the model
    loss, accuracy = train(model, train_data)
    
    # Log the loss and accuracy to wandb
    wandb.log({'train_loss': loss, 'train_accuracy': accuracy})
    
    # Log a sample image to wandb
    image = next(iter(test_data))[0][0]
    wandb.log({'sample_image': wandb.Image(image)})
```

Once you've logged metrics and visualizations to wandb during training, you can view and analyze the results in the wandb dashboard. The wandb dashboard provides a rich set of tools for visualizing and comparing the performance of different models and experiments.

To view your experiments in the wandb dashboard, simply log in to your wandb account and navigate to the project page. You can then view the logged metrics and visualizations for each experiment, as well as compare the results across different experiments.

Here's an example of how to compare the results of two experiments using wandb:

```python
# Initialize wandb for two experiments
wandb.init(project='my-project', name='experiment-1')
train_model(model1, train_data)
wandb.finish()

wandb.init(project='my-project', name='experiment-2')
train_model(model2, train_data)
wandb.finish()

# Compare the results in the wandb dashboard
wandb.agent("my-project", function=compare_experiments, count=2)
```
This example demonstrates how to compare the performance of two different models in the wandb dashboard using the `wandb.agent` function.

In summary, wandb is a powerful and flexible open source tool for visualizing and tracking the performance of machine learning models. By logging metrics and visualizations to wandb during training, you can easily monitor and compare the performance of different models and experiments in the wandb dashboard.

## Model Governance/compliance, performance, monitoring and alerts 🚨

Ensuring your deep learning models comply with regulations and follow best practices is crucial for maintaining trust in your service. Model governance involves tracking, auditing, and controlling access to models and their related artifacts.

### Example: Using MLflow Model Registry for model governance

Register a model with MLflow Model Registry:
```python
import mlflow

# Set tracking URI to connect to the MLflow server
mlflow.set_tracking_uri("http://0.0.0.0:5000")

# Register a model
model_uri = "runs:/<run_id>/model"
model_name = "my_model"
mlflow.register_model(model_uri, model_name)
```
You can also use MLflow's REST API to manage collaborators and their permissions. Refer to the MLflow REST API documentation for details [https://www.mlflow.org/docs/latest/rest-api.html](https://www.mlflow.org/docs/latest/rest-api.html)

### Monitoring 

Monitoring the performance of your deep learning models is crucial for detecting issues and ensuring optimal performance. Setting up alerts can help you respond promptly to any issues that may arise.

As we have seen in the introduction section we can configure alerts using Prometheus and Grafana to send notifications to a communication platform like Slack. For that it is important to configure your model serving API to expose performance metrics in Prometheus format. This can be done using Python libraries like (prometheus_client)[https://github.com/prometheus/client_python] or by instrumenting your model serving API with the appropriate middleware.

Set up alert rules in Grafana to trigger notifications when specific conditions are met, such as when the error rate exceeds a certain threshold or when the latency is too high.

By implementing these MLOps practices for Deep Learning as a Service, you can effectively manage and monitor your deep learning models in production, ensuring optimal performance, scalability, and compliance.