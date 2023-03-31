# Indexing Data with Elasticsearch and Logstash

In this tutorial, we will explore how to index data using Logstash and Elasticsearch. 

## Elasticsearch and Logstash Overview

Elasticsearch is a highly scalable, distributed, and RESTful search and analytics engine based on the Apache Lucene library. It enables users to store, search, and analyze large volumes of data quickly and in near real-time.

Logstash is a data processing pipeline that allows you to collect data from multiple sources, process it, and send it to your desired destination. In our case, Elasticsearch will be the destination for the processed data. Logstash  is part of the ELK stack, which also includes Elasticsearch and Kibana.

The ELK stack is a popular solution for log management and analysis. Here's how the three components work together:

- **Logstash**: Collects data from various sources (such as logs, metrics, and other events) and processes it for further analysis.
- **Elasticsearch**: Stores the processed data in a distributed search and analytics engine, allowing you to search and analyze the data in real-time.
- **Kibana**: Provides a visual interface for exploring and analyzing the data stored in Elasticsearch. It allows you to create custom dashboards and visualizations to better understand your data.

So, Logstash plays a key role in the ELK stack by collecting and processing data from various sources, and then sending it on to Elasticsearch for storage and analysis. It can perform a variety of data transformations and enrichments, such as parsing log files, filtering events, and modifying fields.

## Ingest real time - stream logs with ELK

In this tutorial, we will walk you through setting up a complete ELK stack, which includes Elasticsearch, Logstash, and Kibana. We will be using Filebeat and a python script in order to simulate real time stream logs to send to Logstash and then ingest data into Elasticsearch using Logstash. Lastly, we will visualize the data using Kibana and create a dashboard.


### Architeture overview 

```bash
.
├── docker-compose.yml
├── filebeat
│   └── filebeat.yml
├── logs
│   └── python_logs.log
├── logstash
│   └── logstash.conf
└── send_logs.py
```

### Set up 
Create a new directory for your ELK stack project and navigate to it:
```bash
mkdir elk-stack && cd elk-stack
```

#### Create a stream log event with python 

Here we want to create a Python script that sends a stream of sample logs data to Logstash, which processes the data and makes it available for real-time analytics in Kibana. For this this example, we will use the Python socket library to send sample log data to Logstash over Filebeat. 

Let's start by writing our python script to write our sample logs to a local file, and let Filebeat pick them up and send them to Logstash :

```python title="send_logs.py"

import json
import socket
import time
import random
import os

#sample log messages
sample_logs = [
    {"level": "INFO", "message": "User logged in", "user_id": 1},
    {"level": "DEBUG", "message": "Query executed", "user_id": 3},
]
error_logs = [
    {"level": "ERROR", "message": "Failed to connect to database", "user_id": 2},
    {"level": "ERROR", "message": "Permission denied", "user_id": 4},
]
# local set up 
LOGS_DIR = "./logs"
LOG_FILE = "python_logs.log"

#write log into local file 
def send_log(log):
    if not os.path.exists(LOGS_DIR):
        os.makedirs(LOGS_DIR)

    with open(os.path.join(LOGS_DIR, LOG_FILE), "a") as f:
        f.write(json.dumps(log) + "\n")
        print(f"Sent log: {log}")


def simulate_log_stream():
    while True:
        if random.random() < 0.1:
            log = random.choice(error_logs)
        else:
            log = random.choice(sample_logs)

        send_log(log)
        print(log)
        time.sleep(random.uniform(0.5, 3))

if __name__ == "__main__":
    simulate_log_stream()

```

In this script we send randomly sample log data to Logstash via saving them into a local file with a 10% chance of sending error logs. Now we have to set up a docker ELK stack in order to get and process these logs.

#### Docker-compose ELK stack

Create a new docker-compose.yml file:
```yaml title="docker-compose.yml"
version: '2.2'

services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.11.1
    container_name: elasticsearch
    environment:
      - discovery.type=single-node
      - ES_JAVA_OPTS=-Xms16g -Xmx16g
      - xpack.security.http.ssl.enabled=false
      - xpack.security.transport.ssl.enabled=false
      - xpack.security.enabled=false
    ports:
      - "9200:9200"
      - "9300:9300"
    networks:
      - elk

  logstash:
    image: docker.elastic.co/logstash/logstash:7.11.1
    container_name: logstash
    volumes:
      - /path/to/logstash.conf:/usr/share/logstash/pipeline/logstash.conf
    ports:
      - "5044:5044"
      - "5045:5045"
      - "9600:9600"
    networks:
      - elk
    depends_on:
      - elasticsearch

  kibana:
    image: docker.elastic.co/kibana/kibana:7.11.1
    container_name: kibana
    environment:
    - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
    - ELASTICSEARCH_USERNAME=elastic
    - ELASTICSEARCH_PASSWORD=password
    ports:
      - "5601:5601"
    networks:
      - elk
    depends_on:
      - elasticsearch

  filebeat:
    image: docker.elastic.co/beats/filebeat:7.11.2
    container_name: filebeat
    volumes:
      - ./filebeat/filebeat.yml:/usr/share/filebeat/filebeat.yml
      - ./logs:/logs
    networks:
      - elk
    depends_on:
      - logstash
      - elasticsearch

networks:
  elk:
    driver: bridge
```

#### Configure Filebeat and Logstash

We'll use Filebeat to send logs to Logstash. First, let's write our configuration file for Filebeat:

```bash
touch filebeat.yml
```
Open `filebeat.yml` in your favorite text editor and add the following configuration:

```yaml
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /logs/*.log

output.logstash:
  hosts: ["logstash:5045"]
```

Now, let's configure Logstash for json ingestion. Ineed because we send our logs in json format with python.
First, we need to create a Logstash configuration file:

```bash
cd .. && mkdir logstash && cd logstash
touch logstash.conf
```

Open `logstash.conf` in a text editor and add the following configuration:

```lua
input {
  beats {
    port => 5045
  }
}

filter {
  json {
    source => "message"
    target => "log"
  }
}

output {
  elasticsearch {
    hosts => ["elasticsearch:9200"]
    index => "python-logs-%{+YYYY.MM.dd}"
  }
  stdout {
    codec => rubydebug
  }
}
```

### Process stream data with Elastic and Kibana

Run the Python script to start sending sample log data to Logstash:
```bash
python send_logs.py
```
Verify your elastic cluster is up by running the following command in an other terminal : 
```bash 
curl http://0.0.0.0:9200
```

You should see the following response, it means your Elastic cluster is up and running : 

```bash 
{
  {
  "name" : "9e18531663e6",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "7NZ-LiVhRXyOV-LSH1Vk7A",
  "version" : {
    "number" : "7.11.1",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ff17057114c2199c9c1bbecc727003a907c0db7a",
    "build_date" : "2021-02-15T13:44:09.394032Z",
    "build_snapshot" : false,
    "lucene_version" : "8.7.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

We can also check if the index of our python log data has been created by running this command into your terminal :

```bash
curl http://0.0.0.0:9200/_cat/indices?v
```

You should see in your terminal : 
```bash
health status index                           uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .apm-custom-link                ooiOfqILRle56O66Im9n2w   1   0          0            0       208b           208b
green  open   .kibana-event-log-7.11.1-000001 s8iSoKawTfyZwHE0MPNVYw   1   0          1            0      5.6kb          5.6kb
green  open   .kibana_task_manager_1          xaFul7GiSHCR6yXXeUxJ6g   1   0          8          663    125.1kb        125.1kb
yellow open   python-logs-2023.03.31          B97uqvZ-Rtqh4FjuKr4Czw   1   1       2817            0    320.3kb        320.3kb
green  open   .apm-agent-configuration        cZ86_KEyQXevPm31lf48zw   1   0          0            0       208b           208b
green  open   .async-search                   7Ks0qLUERVuWYJFMLi05GA   1   0          0            0      9.6kb          9.6kb
green  open   .kibana_1                       -UhYW3IRSbKqsQqfInPTFQ   1   0         48           21      2.1mb          2.1mb
```

This line means that our log has been processed well and it is now an elasticsearch index 
```bash
yellow open   python-logs-2023.03.31          B97uqvZ-Rtqh4FjuKr4Czw   1   1       2817            0    320.3kb        320.3kb
```

We can now log into kibana and interact with our index in your browser at [http://localhost:5601](http://localhost:5601) 

Go to the `deev tool` interface inside the `Managment` section of the menu then we can perform some queries on our python-log index. 

### Queries, sorting and pagination 

#### Get index and document 
Like before we can visualize all our indexed data by typing this command into our dev tool kibana interface : 
```bash
GET _cat/indices?v
```

You can also get all the index document by running : 
```bash
GET /python-logs-2023.03.31/_search
```

Now let's retrieve the last 5 added documents from our `python-logs-2023.03.31` index:
```bash
GET /python-logs-2023.03.31/_search
{
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "@timestamp": {
        "order": "desc"
      }
    }
  ],
  "size": 5
}
```

We can also search for documents within the specified datetime range in the "python-logs-2023.03.31" index like this : 

```bash
GET python-logs-2023.03.31/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "range": {
            "@timestamp": {
              "gte": "2023-03-31T16:45:00",
              "lte": "2023-03-31T16:55:00"
            }
          }
        }
      ]
    }
  }
}
```

This query uses a range query to filter the documents based on the `@timestamp` field. The `gte` and `lte` parameters specify the range to search within. You can adjust the `datetime` values as needed to search within a different range.

#### Mapping 

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

You can get the automatic mapping of our created index by typing :
```bash 
GET /python-logs-2023.03.31/_mapping
```
Do not forget to replace `python-logs-2023.03.31` by your index name. You should see something like this in your kibana dev tool output console. 

```bash
{
  "python-logs-2023.03.31" : {
    "mappings" : {
      "properties" : {
        "@timestamp" : {
          "type" : "date"
        },
        "@version" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "agent" : {
          "properties" : {
            "ephemeral_id" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "hostname" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "id" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },

            ...
```

#### Pagination 

Pagination is the process of breaking down a large result set into smaller, more manageable chunks or pages. In Elasticsearch, pagination is used when the number of hits returned by a search query is larger than a specified size, which is the maximum number of hits to return in a single response.

For example, if you perform a search on the `python-logs` index, and the search returns 1000 hits, it might not be practical to display all 1000 hits at once. In such cases, pagination allows you to retrieve the hits in smaller chunks or pages, typically 10, 20, or 50 hits at a time, depending on your configuration.

There are several benefits to using pagination in Elasticsearch, including:

- **Faster response times**: Retrieving a smaller set of results at a time can help to reduce the response time of your queries. This is particularly important for large indexes with lots of data.
- **Improved user experience**: Displaying a large number of hits on a single page can be overwhelming for users. Pagination allows you to present the data in a more manageable way, improving the user experience.
- **Reduced resource usage**: When you limit the number of hits returned in a single response, you can reduce the amount of memory and network bandwidth required to handle the response.

In your case, since you have a large number of logs in your `python-logs` index, it might be practical to use pagination to retrieve the logs in smaller chunks. This will help to improve the performance of your queries and provide a better user experience.

For your information, elasticsearch returns the top 10 results by default. To paginate through the results, you can use the "from" and "size" parameters:

```bash
GET /my_index/_search
{
  "query": { ... },
  "from": 10,
  "size": 10,
  "sort": [ ... ]
}
```

In this example, "from" is the starting document index, and "size" is the number of documents to return, let's practice on our `python-log` index 🤓

##### Pagination using a simple match query
```bash
GET python-logs/_search
{
  "query": {
    "match": {
      "message": "error"
    }
  },
  "size": 10,
  "from": 0
}
```
This will return the first 10 results that match the query "error" in the message field.

To get the next 10 results, you would set "from": 10 in the query.

##### Pagination using a range query on a date field
```bash
GET python-logs/_search
{
  "query": {
    "range": {
      "@timestamp": {
        "gte": "2023-03-31T16:45:00",
        "lte": "2023-03-31T16:55:00"
      }
    }
  },
  "size": 10,
  "from": 0
}
```

This will return the first 10 results that have a timestamp between 16:45:00 and 16:55:00 on March 31st, 2023.
Same thing, to get the next 10 results, you would set "from": 10 in the query.

##### Pagination using a terms query and sorting by a numeric field
```bash
GET python-logs/_search
{
  "query": {
    "terms": {
      "log.level": ["error", "warning"]
    }
  },
  "sort": [
    {"log.user_id": "asc"}
  ],
  "size": 10,
  "from": 0
}
```

This will return the first 10 results that have a log.level of `error` or `warning`, sorted by the `log.user_id` field in ascending order.


_____________________________________________________________________________________________________________________________________

## Visualizing real time data in Kibana

### Create index pattern 

In Elasticsearch, an index is a collection of documents that have somewhat similar characteristics. It is used to store and search data. An index pattern, on the other hand, is a way to define the name of an index or a set of indexes that Kibana should use to retrieve data from Elasticsearch.

When Kibana connects to Elasticsearch, it needs to know which index to search for data. This is where index patterns come into play. An index pattern tells Kibana which index or set of indexes it should use to retrieve data from Elasticsearch.

Index patterns can be defined using wildcards, which allow for more flexibility in naming indexes. For example, you could define an index pattern that matches all indexes that start with "logstash-", such as "logstash-*". This would allow Kibana to search for data across all indexes that match the pattern, regardless of their specific names.

Index patterns are important because they allow Kibana to map data to visualizations, dashboards, and searches. When you create a visualization or dashboard in Kibana, you need to specify which index pattern the data should come from. This allows Kibana to retrieve the data from Elasticsearch and display it in the appropriate visualizations.

Overall, index patterns are a crucial part of the Kibana interface, as they allow users to easily search, visualize, and analyze data stored in Elasticsearch.

Let's create the index pattern we need : 

1. Open Kibana in your web browser by navigating to [http://localhost:5601](http://localhost:5601)
2. Go to the `Stack Management` tab in the left sidebar and click on the `Kibana` section on `Index Patterns`
3. Click the `Create index pattern` button
4. Enter the `Index pattern name`, four our case let's enter `python-logs*` field and click `Next`
5. Select the `@timestamp` field as the `Time Filter field name` and click `Create index pattern`


### Visualize our indexed data 

Now that we have our index reconize by Kibana let's create some graphics

1. On the menu through the Kibana section go to the `Discover` tab in the left sidebar to explore your indexed data, you can notice that you can change the time interval with the calendar button on the top right. This is just a general visualization grah of our data, you can refresh it and see the data in real time.
2. To create visualizations and dashboards, go to the `Dashboard` section in the left sidebar

... 

explain graph types 
... 


![Screenshot](../img/kibana_dashboard_final.png)



After running the modified Python script and sending the logs to Logstash, follow these steps to create a visualization in Kibana that displays the error logs:

Open Kibana in your web browser by navigating to http://localhost:5601.
Ensure that the index pattern tcp_data is already created, as described in previous tutorials.
Go to the "Visualize" tab in the left sidebar and click the "Create visualization" button.
Select a visualization type, such as "Pie" or "Vertical bar."
Choose the tcp_data index pattern as the data source.
Now, configure the visualization to display only error logs:

In the "Buckets" section, click the "Add" button next to "Split slices" (for a pie chart) or "Split series" (for a vertical bar chart).
Select the "Filters" aggregation.
Click "Add Filter" and configure the filter with the following settings:
Field: level.keyword
Operator: is
Value: ERROR
Click the "Save" button to apply the filter.
In the "Buckets" section, you can also add another aggregation to split the chart based on other fields (e.g., user_id, message).
Click the "Update" button at the top-right corner of the screen to see the updated visualization.
You have now created a visualization in Kibana that displays only error logs.

To create a dashboard with this visualization:

Go to the "Dashboard" tab in the left sidebar and click the "Create dashboard" button.
Click the "Add" button at the top-right corner of the screen.
Select the visualization you just created.
Click "Save" in the top-right corner of the screen, and provide



Now you have successfully set up an ELK stack with Filebeat, ingested CSV files using Logstash, and visualized the data in Kibana!

Feel free to explore Kibana's features, such as creating various types of visualizations, adding them to dashboards, and configuring alerts to stay informed about your data.

Remember to update the configurations in the tutorial to match the actual data and file paths you're working with. And don't forget to monitor your Docker containers to ensure they are running smoothly.
_____________________________________________________________________________________________________________________________________



## Analyzers

Analyzers are responsible for the text analysis process in Elasticsearch. They consist of three components:

- `Character filters`: Process the input text stream before tokenization, e.g., removing HTML tags.
- `Tokenizer`: Breaks the input text into individual tokens.
- `Token filters`: Process tokens after tokenization, e.g., lowercasing, stemming, stopwords removal.

Elasticsearch offers built-in analyzers (e.g., standard, whitespace, simple), or you can create custom analyzers.


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

```lua
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














_____________________________________________________________________________________________________________________________________


## Data ingestion pipeline for apache web server logs processing

In this part, we will add to our existing configuration a data ingestion pipeline to process apache web server logs with our Logstash service and visualize them using Kibana. This project will help you gain hands-on experience with the Elastic Stack by processing real-world data.

### Project architecture overview

```bash
.
├── data
│   └── apache_logs.txt
├── docker-compose.yml
├── filebeat
│   └── filebeat.yml
├── logs
│   └── python_logs.log
├── logstash
│   └── logstash-tcp.conf
├── send_logs.py
└── web_server_logs
    └── logstash-apache.conf
```

### Download Sample Web Server Logs

First, download a sample web server log file from the following link:

[Sample Web Server Log](https://raw.githubusercontent.com/elastic/examples/master/Common%20Data%20Formats/apache_logs/apache_logs)

Save the file as apache_logs.txt in a new directory for this project.

### Set Up Logstash

Create a new Logstash configuration file for ingesting the web server logs:

```bash
mkdir web_server_logs
cd web_server_logs
touch logstash-apache.conf
```

Open `logstash-apache.conf` in a text editor and add the following configuration:

```lua
input {
  file {
    path => "/data/apache_logs.txt"
    start_position => "beginning"
    sincedb_path => "/dev/null"
  }
}

filter {
  grok {
    match => { "message" => "%{COMBINEDAPACHELOG}" }
  }
  date {
    match => [ "timestamp", "dd/MMM/yyyy:HH:mm:ss Z" ]
    target => "@timestamp"
    remove_field => "timestamp"
  }
  geoip {
    source => "clientip"
  }
}

output {
  elasticsearch {
    hosts => ["elasticsearch:9200"]
    index => "web_server_logs"
  }
}
```

Replace `/data/apache_logs.txt` with the actual path to the `apache_logs.txt` file if you have changed the architecture.

### Update Docker Compose Configuration

Update your docker-compose.yml file to include the new Logstash configuration file for the web server logs:

```yaml
logstash:
  # ...
  volumes:
    # ...
    - ./web_server_logs/logstash-apache.conf:/usr/share/logstash/pipeline/logstash-apache.conf
    - /data/apache_logs.txt:/apache_logs.txt
  # ...
```
Replace `/data/apache_logs.txt` with the actual path to the `apache_logs.txt` file.

#### Restart the Logstash container to apply the new configuration:

```bash
docker-compose down
docker-compose up -d
```

### Visualize Web Server Logs in Kibana

Open Kibana in your web browser by navigating to [http://localhost:5601](http://localhost:5601).

### Create a new index pattern named web_server_logs.
Go to the `Discover` tab in the left sidebar to explore the indexed data.

Create Visualizations
To create visualizations for the web server logs, follow these steps:

Go to the "Visualize" tab in the left sidebar and click the "Create visualization" button.
Select a visualization type, such as "Pie" or "Vertical bar."
Choose the web_server_logs index pattern as the data source.
Create various visualizations based on different aspects of the web server logs, such as:

HTTP response status codes (use the response field)
Top client IP addresses (use the clientip field)
Most frequently requested resources (use the request field)
Number of requests over time (use the @timestamp field)
Geographic distribution of client IP addresses (use the geoip.location field)
Create a Dashboard
After creating the visualizations, you can compile them into a dashboard:

Go to the "Dashboard" tab in the left sidebar and click the "Create dashboard" button.
Click the "Add" button at the top-right corner

_____________________________________________________________________________________________________________________________________
## Conclusion
This comprehensive tutorial covers setting up a Logstash and Kibana architecture using Docker, using Filebeat with Logstash, indexing CSV files with Logstash, and visualizing indexed data using Kibana. With this knowledge, you can effectively collect, process, and visualize data in the Elasticsearch, Logstash, and Kibana stack.
