# Introduction to Elasticsearch

Elasticsearch is an open-source, document-based NoSQL database that is designed for full-text search and analytics. Elasticsearch is built on top of the Lucene search engine and is **optimized for fast search and retrieval of unstructured data**.

## Elastic architecture introduction 

Elasticsearch is a distributed, document-oriented NoSQL database that is optimized for search and analytics. It is built on top of the Lucene search engine and is designed to scale horizontally across multiple nodes and clusters.

### Cluster, nodes and replicas 

Nodes are individual instances of Elasticsearch that are part of a cluster. Each node stores a subset of the data in the cluster and is responsible for processing search and indexing requests for that data. Nodes can be added or removed from a cluster dynamically, allowing for easy scalability and fault tolerance.

A cluster is a collection of nodes that work together to store and process data. Elasticsearch clusters are designed to be highly available and fault tolerant, with built-in features for data replication and failover. Clusters can scale horizontally by adding more nodes to the cluster, allowing for increased processing power and storage capacity.

Sharding is the process of partitioning data across multiple nodes in a cluster. When a document is indexed in Elasticsearch, it is assigned to a specific shard based on a hashing algorithm that takes into account the document's ID. By default, each index in Elasticsearch is divided into five primary shards, with each shard having one or more replicas. This allows Elasticsearch to distribute the workload across multiple nodes, improving search and indexing performance.

The number of shards and replicas can be configured for each index based on the size and search requirements of the data. For example, a large index with a high write throughput might require more primary shards to distribute the data more evenly across nodes, while a smaller index with a lower write throughput might require fewer primary shards to reduce the overhead of managing multiple shards.

<center>
![Screenshot](../img/elasticcluster.png)
</center>

### Index

An index is like a database in a traditional relational database system. It is a logical container for one or more documents that share a similar structure and are stored together.

When you create an index in Elasticsearch, you can specify the mapping for the fields that the documents in the index will contain. The mapping defines the data types and formats for the fields, which allows Elasticsearch to index and search the data more efficiently.

You can also configure the number of primary shards that the index should have, which determines how the data in the index is distributed across nodes in the Elasticsearch cluster. This allows Elasticsearch to scale horizontally as the size of the index grows.

In addition, you can configure the number of replica shards for the index, which provide redundancy and allow for failover in case a primary shard becomes unavailable.

<center>
![Screenshot](../img/elasticindex.png)
</center>

Overall, an index in Elasticsearch is a distributed, document-oriented NoSQL database that uses nodes, clusters, and sharding to provide scalability, fault tolerance, high availability and provides a way to organize and search data efficiently.

### Overall architecture 

The diagram shows a simplified architecture for an Elasticsearch cluster with three nodes. Each node is represented by a rectangular box and is labeled with its unique node name, IP address, and port number.

<center>
![https://www.golinuxcloud.com/wp-content/uploads/2020/01/elasticsearch-cluster-architecture.jpg](https://www.golinuxcloud.com/wp-content/uploads/2020/01/elasticsearch-cluster-architecture.jpg)
</center>

The nodes are connected to each other through a network, represented by the gray lines connecting the boxes. This network allows the nodes to communicate with each other and share data.

The Elasticsearch cluster is managed by a master node, which is responsible for coordinating the cluster and maintaining its state. In the diagram, the master node is indicated by the green box labeled "Master-eligible node."

The other nodes in the cluster are known as data nodes, and they are responsible for storing and indexing the data. In the diagram, the data nodes are indicated by the yellow boxes labeled "Data node."

Each data node stores a subset of the data in the cluster, and the data is distributed across the nodes using a technique called sharding. Each shard represents a portion of the data and is stored on a separate data node.

The client nodes, represented by the blue boxes labeled "Client node," are used to interact with the cluster and submit search and indexing requests. Client nodes do not store any data themselves, but they communicate with the data nodes to retrieve and manipulate the data.

Finally, the external world is represented by the orange box labeled "External clients," which can be any application or user that needs to interact with the Elasticsearch cluster.

Overall, the architecture diagram shows how an Elasticsearch cluster is composed of multiple nodes working together to store, index, and search data efficiently.

## Installation with Docker

To install Elasticsearch, we can use Docker, a containerization platform that simplifies the process of installing and running software applications. Install Docker on your machine if you haven't already.

Open a command prompt and run the following command to download and run the Elasticsearch Docker image:
```bash
docker run -p 9200:9200 -p 9300:9300 -d -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.14.0
```

- `docker run`: This command tells Docker to run a container from an image.
- `-p` 9200:9200: This option maps port 9200 in the container to port 9200 on the host machine, allowing us to access Elasticsearch on port - `9200` from outside the container.
- `-p` 9300:9300: This option maps port 9300 in the container to port 9300 on the host machine, allowing nodes in the Elasticsearch cluster to communicate with each other on port 9300.
- `-d`: This option runs the container in detached mode, which means it runs in the background and doesn't attach to the terminal. This allows us to continue using the terminal while the container is running.
- `-e` "discovery.type=single-node": This option sets an environment variable in the container called discovery.type to single-node. This tells Elasticsearch to start as a single-node cluster, which is useful for testing or development purposes.
- `docker.elastic.co/elasticsearch/elasticsearch:7.14.0`: This is the name and version of the Elasticsearch image we want to run.

When you run this command, Docker will download the Elasticsearch image from Docker Hub (unless it's already downloaded), create a container from the image, and start Elasticsearch running inside the container. The container will be accessible on port 9200 and 9300, and Elasticsearch will be running as a single-node cluster 🥳


### Test your installation 

#### Cluster info 
You can test if the elasticsearch container is running by run this command into your terminal (the `jq` command ) : 
```bash 
curl 0.0.0.0:9200/_cluster/health | jq
```

You should see this : 
```bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   390  100   390    0     0    308      0  0:00:01  0:00:01 --:--:--   308
{
  "cluster_name": "docker-cluster",
  "status": "green",
  "timed_out": false,
  "number_of_nodes": 1,
  "number_of_data_nodes": 1,
  "active_primary_shards": 1,
  "active_shards": 1,
  "relocating_shards": 0,
  "initializing_shards": 0,
  "unassigned_shards": 0,
  "delayed_unassigned_shards": 0,
  "number_of_pending_tasks": 0,
  "number_of_in_flight_fetch": 0,
  "task_max_waiting_in_queue_millis": 0,
  "active_shards_percent_as_number": 100
}
```

This command returns information about the overall health of the Elasticsearch cluster. It provides information about the number of nodes in the cluster, the status of each node, the number of shards and replicas, and the overall status of the cluster. The response includes a variety of metrics such as the number of unassigned shards, the number of active and inactive primary shards, and the status of the cluster's overall health.

#### Nodes info
```bash 
curl -X GET "http://0.0.0.0:9200/_cat/nodes?v"
```
This command returns information about the nodes in the Elasticsearch cluster. It provides a summary of each node, including its IP address, node ID, and whether it is currently active or not. The response also includes a variety of metrics such as the number of open file descriptors, the amount of disk space used, and the amount of heap memory used.


## Data Modeling in Elasticsearch

Elasticsearch stores data as JSON-like documents, which can be nested and hierarchical. The data model in Elasticsearch is flexible, allowing for easy changes to the schema. 

**Documents are the primary storage structure in Elasticsearch. Each document contains reserved fields (the document's metadata), such as**:

<center>
![Screenshot](../img/elasticdoc.png)
</center>


- `_index`: where the document resides
- `_type`: the type of document it represents (database)
- `_id`: a unique identifier for the document
- `_source`: the data in the form of a dictionary

### `json` format

JSON (JavaScript Object Notation) is a lightweight data interchange format that is easy for humans to read and write and easy for machines to parse and generate. It consists of key-value pairs, with each pair separated by a comma and enclosed in curly braces. Here's an example of a simple JSON document:

```json
{
  "name": "John Smith",
  "age": 35,
  "city": "New York"
}
```
In Elasticsearch, JSON is used as the primary format for documents that are stored and indexed in the database. Each document is represented as a JSON object, with fields that describe the properties of the document.



## Indexing data in Elasticsearch

### Create index from `json` file 
Let's create a bash script in order to insert few indices from json files for and play with them:  

```bash title="insert_data.sh"
curl -s -H "Content-Type: application/x-ndjson" -XPOST localhost:9200/receipe/_bulk --data-binary "@receipe.json" &&\
printf "\n✅ Insertion receipe index to elastic node OK ✅ "

curl -s -H "Content-Type: application/x-ndjson" -XPOST localhost:9200/accounts/docs/_bulk --data-binary "@accounts.json"
printf "\n✅ Insertion accounts index to elastic node OK ✅ "

curl -s -H "Content-Type: application/x-ndjson" -XPOST localhost:9200/movies/_bulk --data-binary "@movies.json"
printf "\n✅ Insertion movies index to elastic node OK ✅ "

curl -s -H "Content-Type: application/x-ndjson" -XPOST localhost:9200/products/_bulk --data-binary "@products.json"
printf "\n✅ Insertion products index to elastic node OK ✅ "
```

- `curl`: a command-line tool for sending HTTP requests and receiving HTTP responses from a server.
- `-s`: a flag that tells curl to operate silently, i.e., not to show the progress meter or any error messages.
- `-H` "Content-Type: application/x-ndjson": a header that sets the content type of the request to application/x-ndjson. This tells - Elasticsearch that the data being sent in the request body is in NDJSON format.
- `-XPOST`: a flag that tells curl to send a POST request to the specified URL.
- `localhost:9200/receipe/_bulk`: the URL of the Elasticsearch endpoint to which the request is being sent. In this case, it's the _bulk API for the receipe index on the local Elasticsearch instance running on port 9200.
- `--data-binary "@receipe.json"`: the request body, which is specified as a binary data file (@ symbol followed by the filename in this case receipe) in NDJSON format. The --data-binary flag tells curl to send the data as is, without any special interpretation.

Overall, this command is sending a bulk request to Elasticsearch to index data contained in the receipe.json file into the receipe index. The data is in NDJSON format, which is a format that Elasticsearch can parse and process efficiently.

### Return format of query 

<center>
![Screenshot](../img/elastic_return.png)
</center>

## CRUD Operations in Elasticsearch

Elasticsearch supports CRUD (Create, Read, Update, and Delete) operations for manipulating data. Here are some examples of how to perform CRUD operations in Elasticsearch:

## Querying Elasticsearch data



## Aggregation in Elasticsearch
