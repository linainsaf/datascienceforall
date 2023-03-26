# Indexing Data with Elasticsearch and Logstash

In this tutorial, we will explore how to index data using Elasticsearch and Logstash. We will also cover essential topics such as indexation, retrieval, deletion of documents, components of an index, text analysis, search indexation, mapping, configuration, analyzers, queries, sorting, and pagination.

## Elasticsearch and Logstash Overview

Elasticsearch is a highly scalable, distributed, and RESTful search and analytics engine based on the Apache Lucene library. It enables users to store, search, and analyze large volumes of data quickly and in near real-time.

Logstash is a data processing pipeline that allows you to collect data from multiple sources, process it, and send it to your desired destination. In our case, Elasticsearch will be the destination for the processed data.

## Indexation, Retrieval, and Deletion of Documents

### Components of an Index

An index in Elasticsearch is a logical namespace to organize data, similar to a database in a relational DBMS. It contains several primary components:

- Documents: JSON objects containing the data.
- Types: Deprecated since version 7.0.0, previously used to define different types of documents within an index.
- Shards: Indices can be split into smaller units called shards, enabling horizontal scaling.
- Replicas: A copy of a shard, providing high availability and fault tolerance.

### Text Analysis and Search Indexation

Text analysis is the process of converting unstructured text data into a structured format to enable efficient searching and analysis. Elasticsearch uses an inverted index to perform fast and efficient full-text searches. During indexation, Elasticsearch performs the following steps:

- Tokenization: Breaking text into individual terms (tokens).
- Lowercasing: Converting all tokens to lowercase.
- Filtering: Removing specific tokens (e.g., stopwords, short tokens) or applying other transformations (e.g., stemming, synonyms).

### Mapping and Configuration of Index
Mapping is the process of defining how a document and its fields are stored and indexed in Elasticsearch. It includes specifying the data type (e.g., text, keyword, date) and configuring indexing options for each field.

To create an index with custom mapping, use the following command:

```bash
PUT /my_index
{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 2
  },
  "mappings": {
    "properties": {
      "field1": { "type": "text" },
      "field2": { "type": "keyword" },
      "field3": { "type": "date" }
    }
  }
}
```

## Analyzers

Analyzers are responsible for the text analysis process in Elasticsearch. They consist of three components:

- Character filters: Process the input text stream before tokenization, e.g., removing HTML tags.
- Tokenizer: Breaks the input text into individual tokens.
- Token filters: Process tokens after tokenization, e.g., lowercasing, stemming, stopwords removal.

Elasticsearch offers built-in analyzers (e.g., standard, whitespace, simple), or you can create custom analyzers.

## Queries and Sorting (continued)
You can use the Query DSL (Domain Specific Language) to write complex queries in JSON format. You can check the intro part of the course for some examples. 

## Pagination
Elasticsearch returns the top 10 results by default. To paginate through the results, you can use the "from" and "size" parameters:

```bash
GET /my_index/_search
{
  "query": { ... },
  "from": 10,
  "size": 10,
  "sort": [ ... ]
}
```

In this example, "from" is the starting document index, and "size" is the number of documents to return.

Please note that Elasticsearch's deep pagination could be inefficient and cause performance problems. For large result sets, consider using the "search_after" parameter or the Scroll API for more efficient handling.

## Elasticsearch, Logstash, and Kibana (ELK) Stack Setup using Docker

Create a new directory for the project, and inside the directory, create a docker-compose.yml file with the following content:
```yaml
version: '3.2'
services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.15.2
    environment:
      - "discovery.type=single-node"
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ports:
      - "9200:9200"
    networks:
      - elk

  logstash:
    image: docker.elastic.co/logstash/logstash:7.15.2
    ports:
      - "5044:5044"
    volumes:
      - ./logstash/config:/usr/share/logstash/config
      - ./logstash/pipeline:/usr/share/logstash/pipeline
    networks:
      - elk
    depends_on:
      - elasticsearch

  kibana:
    image: docker.elastic.co/kibana/kibana:7.15.2
    ports:
      - "5601:5601"
    networks:
      - elk
    depends_on:
      - elasticsearch

networks:
  elk:
    driver: bridge
```
This configuration sets up an Elasticsearch, Logstash, and Kibana instance with a bridge network named 'elk'.

### Using Filebeat with Logstash

Create a new Filebeat configuration file filebeat.yml in the project directory with the following content:
```yml
filebeat.inputs:
- type: log
  paths:
    - /path/to/your/logs/*.log

output.logstash:
  hosts: ["localhost:5044"]
```
Replace /path/to/your/logs/ with the actual path to your log files.

### Using Logstash for Indexing CSV Files

Create a new directory logstash inside the project directory, and within it, create two more directories: config and pipeline. Inside the config directory, create a logstash.yml file with the following content:
```yml 
http.host: "0.0.0.0"
path.config: /usr/share/logstash/pipeline
```
Inside the pipeline directory, create a csv-pipeline.conf file with the following content:

```ruby
input {
  beats {
    port => 5044
  }
}

filter {
  csv {
    separator => ","
    columns => ["column1", "column2", "column3"]
  }
  date {
    match => ["column3", "ISO8601"]
    target => "@timestamp"
  }
}

output {
  elasticsearch {
    hosts => ["elasticsearch:9200"]
    index => "csv-data-%{+YYYY.MM.dd}"
  }
}
```

Replace column1, column2, and column3 with your CSV file's actual column names. If your CSV has a date column, specify its format in the date filter.

### Start the ELK stack and Filebeat using Docker:
```bash

docker-compose up -d
docker run --name=filebeat --volume="$(pwd)/filebeat.yml:/usr/share/filebeat/filebeat.yml:ro" --volume="/path/to/your/logs:/usr/share/filebeat/logs:ro" --net=project_directory_elk docker.elastic.co/beats/filebeat:7.15.2
```
Replace /path/to/your/logs with the actual path to your log files, and project_directory with the name of your project directory.

Wait for the stack to initialize, then open Kibana in your browser at http://localhost:5601.
Go to the "Management" tab, and then to "Index Patterns."
Click on "Create index pattern" and enter csv-data-* as the pattern name. Click "Next step," select @timestamp as the Time Filter field, and click "Create index pattern."
To view the indexed data, go to the "Discover" tab, and you should see the indexed CSV data in Kibana.

## Conclusion
This comprehensive tutorial covers setting up a Logstash and Kibana architecture using Docker, using Filebeat with Logstash, indexing CSV files with Logstash, and visualizing indexed data using Kibana. With this knowledge, you can effectively collect, process, and visualize data in the Elasticsearch, Logstash, and Kibana stack.
