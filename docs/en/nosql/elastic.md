# Introduction to Elasticsearch

Elasticsearch is an open-source, document-based NoSQL database that is designed for full-text search and analytics. Elasticsearch is built on top of the Lucene search engine and is **optimized for fast search and retrieval of unstructured data**.

## Installation with Docker

To install Elasticsearch, we can use Docker, a containerization platform that simplifies the process of installing and running software applications. Follow these steps to install Elasticsearch with Docker:

1. Install Docker on your machine if you haven't already.
2. Open a command prompt and run the following command to download and run the Elasticsearch Docker image:
```bash
docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.14.0
```
3. Elasticsearch should now be running on your machine on port 9200.

Data Modeling in Elasticsearch

Elasticsearch stores data as JSON-like documents, which can be nested and hierarchical. The data model in Elasticsearch is flexible, allowing for easy changes to the schema. When designing a data model in Elasticsearch, it's important to consider the search and retrieval requirements of the application, as well as the size and complexity of the data.

CRUD Operations in Elasticsearch

Elasticsearch supports CRUD (Create, Read, Update, and Delete) operations for manipulating data. Here are some examples of how to perform CRUD operations in Elasticsearch: