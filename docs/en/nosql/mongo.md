# Mongo DB

MongoDB is a popular NoSQL database that provides high performance, high availability, and easy scalability. It is a document-oriented database, which means that it stores data in a semi-structured format called BSON (Binary JSON). **The flexible schema of MongoDB allows you to store complex data without the constraints of a rigid schema, making it a suitable choice for various types of applications**.

## Introduction to MongoDB Architecture

<center>
![Screenshot](https://webimages.mongodb.com/_com_assets/cms/kz7ehw006zlb69h6k-image1.png?auto=format%252Ccompress)
</center>

### MongoDB Components

The MongoDB architecture consists of several components, including:

- Query Router (mongos): The query router is responsible for routing client requests to the appropriate shard(s). It is a lightweight, stateless component that uses the cluster's metadata to determine the location of the data. When a client sends a query to the mongos, it determines which shard(s) hold the relevant data and forwards the query accordingly.
- Config Server: The config server stores metadata about the cluster, such as information about the shards, the distribution of data, and the state of the cluster. In a sharded cluster, there are usually three config servers to ensure high availability and data consistency. The config servers use a replica set to store the metadata redundantly and synchronize changes among themselves.
- Shard: A shard is a container for a portion of the data in the cluster. Each shard can be a standalone mongod instance or a replica set. Sharding is a technique used to distribute data across multiple servers, allowing MongoDB to scale horizontally and support very large datasets. Shards are responsible for storing and managing the actual data.
- Replica Set: A replica set is a group of mongod instances that maintain the same data set. It provides redundancy and high availability by replicating data across multiple nodes. In a replica set, one node is the primary node, which receives all write operations, while the other nodes are secondary nodes that replicate the primary's data. If the primary node fails, an election process occurs to choose a new primary from the available secondary nodes.

### MongoDB Data Model

MongoDB uses a document data model, which organizes data in a hierarchical structure as follows:

- Database: A MongoDB server can host multiple databases. Each database is a separate container for collections and provides a logical way to organize data.
- Collection: A collection is a group of documents in a database. Collections are similar to tables in relational databases, but they do not enforce a rigid schema. Documents within a collection can have different fields and structures, providing a flexible data model.
- Document: A document is the basic unit of data in MongoDB. It is a BSON object that can store various types of data, such as strings, numbers, dates, and arrays. Documents are analogous to rows in relational databases but can store complex, nested data structures.

By understanding the components and data model of MongoDB, you can better appreciate how the database is designed to provide high performance, availability, and scalability. As you explore MongoDB further, you will learn more about its features, such as indexing, aggregation, and transactions, and how they can be leveraged to build powerful and flexible applications.

In this section, we will explore how to work with MongoDB using Python and the `PyMongo` library. We will also demonstrate how to set up MongoDB using Docker. By the end of this tutorial, you will have a thorough understanding of MongoDB, Python, and Docker, and you will be able to build applications using these technologies.

## Setting Up MongoDB with Docker

To set up MongoDB using Docker, first, ensure that you have Docker installed on your system. If you don't have it installed, follow the instructions on the Docker website: https://docs.docker.com/get-docker/

Once Docker is installed, pull the latest MongoDB Docker image:

```bash
docker pull mongo
```

Next, run a MongoDB container:

```bash
docker run --name my-mongo -p 27017:27017 -d mongo
```

This command creates and starts a new MongoDB container named `my-mongo` and maps the host port `27017` to the container's port `27017`. The -d flag runs the container in detached mode, so it runs in the background.

To interact with MongoDB using Python, you'll need to install the PyMongo library. Install it using pip:

```bash
pip install pymongo
```

## Connecting to MongoDB with Python

Create a Python file and import the pymongo module:

```python
from pymongo import MongoClient
```
Connect to your MongoDB server using the following code:
```python
client = MongoClient('mongodb://localhost:27017/')
```

Now, you can access the MongoDB server through the client object.

## Basic CRUD Operations with PyMongo

Before diving into CRUD operations, let's create a database and a collection:

```python
db = client.mydb
collection = db.mycollection
```

### Create (Insert):

To insert a document into a collection, use the insert_one method:

```python
document = {"name": "John Doe", "email": "john.doe@example.com", "age": 30}
result = collection.insert_one(document)
print("Inserted document ID:", result.inserted_id)
```
To insert multiple documents at once, use the insert_many method:
```python
documents = [
    {"name": "Alice", "email": "alice@example.com", "age": 25},
    {"name": "Bob", "email": "bob@example.com", "age": 35}
]
result = collection.insert_many(documents)
print("Inserted document IDs:", result.inserted_ids)
```

### Read (Query):

To retrieve a single document from a collection, use the find_one method:

```python
query = {"name": "John Doe"}
document = collection.find_one(query)
print(document)
```

To retrieve multiple documents, use the find method:
```python
query = {"age": {"$gt": 25}}
documents = collection.find(query)

for doc in documents:
    print(doc)
```

### Update:

To update a single document, use the update_one method:

```python
query = {"name": "John Doe"}
update = {"$set": {"age": 31}}
result = collection.update_one(query, update)
print("Modified document count:", result.modified_count)
```

To update multiple documents, use the update_many method:

```python
query = {"age": {"$gt": 25}}
update = {"$inc": {"age": 1}}
result = collection.update_many(query, update)
print("Modified document count:", result.modified_count)
```

### Delete:

To delete a single document, use the delete_one method:

```python
query = {"name": "John Doe"}
result = collection.delete_one(query)
print("Deleted document count:", result.deleted_count)
```

To delete multiple documents, use the delete_many method:

```python
query = {"age": {"$gt": 25}}
result = collection.delete_many(query)
print("Deleted document count:", result.deleted_count)
```

### Querying and Filtering Data

In addition to basic CRUD operations, PyMongo provides advanced querying and filtering capabilities.

#### Logical Operators

You can use logical operators like `$and`, `$or`, `$not`, and `$nor` to combine query conditions:

```python
query = {
    "$and": [
        {"age": {"$gt": 25}},
        {"email": {"$regex": "@example\.com$"}}
    ]
}
documents = collection.find(query)

for doc in documents:
    print(doc)
```

#### Projection

To limit the fields returned in query results, use a projection:

```python
query = {"age": {"$gt": 25}}
projection = {"_id": 0, "name": 1, "email": 1}
documents = collection.find(query, projection)

for doc in documents:
    print(doc)
```

#### Sorting

To sort query results, use the sort method:

```python
query = {"age": {"$gt": 25}}
documents = collection.find(query).sort("name", pymongo.ASCENDING)

for doc in documents:
    print(doc)
```

## Indexing and Aggregation

An index in MongoDB is a data structure that allows the database to efficiently locate and access documents based on the values of specific fields. Indexes improve the performance of read operations, particularly for large datasets, by reducing the number of documents that must be examined to satisfy a query. However, they come with some trade-offs, as they require additional storage space and can slow down write operations.

In MongoDB, the default index is the `_id` field, which is automatically indexed to ensure the uniqueness of each document within a collection. However, you can also create custom indexes on other fields to optimize your queries.

There are several types of indexes in MongoDB:

- Single Field Index: This type of index is created on a single field within the documents. It can be either ascending (1) or descending (-1).
- Compound Index: A compound index is created on multiple fields within the documents. The order of the fields in the index definition determines the order in which the index is sorted.
- Multikey Index: A multikey index is created on fields that hold arrays. MongoDB creates separate index entries for each element in the array field, allowing efficient queries on array data.
- Text Index: This type of index is created on string content and is optimized for full-text search operations. A text index allows you to perform searches for words or phrases within the indexed fields.
- 2dsphere Index: A 2dsphere index is used for geospatial queries based on GeoJSON objects. This index type allows you to efficiently perform geospatial operations such as finding nearby points or calculating distances between points.
- 2d Index: A 2d index is created for legacy coordinate pairs and is used for planar (flat) geometry operations.
- Hashed Index: A hashed index is created on hashed values of the specified field. This type of index is used to support sharding and enables MongoDB to partition data across shards.

When creating an index in MongoDB, you should carefully consider your application's query patterns and performance requirements. Keep in mind that while indexes can significantly improve read performance, they can also consume additional storage space and impact write performance. To achieve the best balance, it is essential to analyze your application's needs and create indexes that cater to the most common and performance-critical queries.

### Indexing

To create an index on a field, use the create_index method:

```python
index_name = collection.create_index("email", unique=True)
```

### Aggregation

To perform aggregation operations, use the aggregate method:

```python
pipeline = [
    {"$match": {"age": {"$gt": 25}}},
    {"$group": {"_id": "$age", "count": {"$sum": 1}}}
]

results = collection.aggregate(pipeline)

for result in results:
    print(result)
```

## Wrap up

In this tutorial, we have covered the process of setting up MongoDB with Docker and using Python to interact with MongoDB through the PyMongo library. We have explored basic CRUD operations, advanced querying and filtering, and indexing and aggregation. With these concepts and examples, you are now well-equipped to build applications using MongoDB and Python. As you continue to develop your skills, consider exploring other features offered by MongoDB, such as transactions, geospatial queries, and change streams.