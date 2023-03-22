# Neo4j

In this section, we will cover how to set up a Neo4j graph database using Docker, how to connect it with Python, and perform various operations like creating nodes, relationships, and executing Cypher queries. By the end of this tutorial, you will have a good understanding of how to use Neo4j with Python for managing graph databases.

## Prerequisites

- Docker installed on your machine
- Basic knowledge of Python and graph theory 

## Environment set up

### Running a Neo4j container

```bash
docker run     
    \--name my_neo4j     
    \-p7474:7474 -p7687:7687     
    \-v ~/neo4j_data:/data     
    \-e NEO4J_AUTH=neo4j/password     
    \-d neo4j
```
This command will create and run a new Docker container called `my_neo4j` and expose ports 7474 (for the Neo4j Browser) and 7687 (for the Bolt protocol) on your local machine. The environment variable "NEO4J_AUTH" sets the initial password for the "neo4j" user and it runs in a background via `-d` option.

Once the container is running, you can access the Neo4j Browser at [http://localhost:7474](http://localhost:7474). Log in with the username "neo4j" and the password you set earlier.

### Installing the necessary Python libraries
In this tutorial, we will use the official Python driver for Neo4j called "neo4j". To install it, run the following command:

```
pip install neo4j
```

### Connecting Python to Neo4j

To connect Python to the Neo4j database, you need to create a driver instance and authenticate with the Neo4j server. Replace <password> with the password you set earlier when running the Docker container:

```python
from neo4j import GraphDatabase

uri = "bolt://localhost:7687"

user = "neo4j"
password = "<password>"

driver = GraphDatabase.driver(uri, auth=(user, password))

def run_query(query):
    with driver.session() as session:
        result = session.run(query)
        return result
```

Let's create some sample nodes and relationships using Python and Neo4j. In this example, we will create a small graph of persons and their relationships.

## Creating nodes

```python
person1 = "CREATE (p:Person {name: 'Alice', age: 30})"
person2 = "CREATE (p:Person {name: 'Bob', age: 25})"
person3 = "CREATE (p:Person {name: 'Charlie', age: 35})"

run_query(person1)
run_query(person2)
run_query(person3)
```

### Create relationships
```python
relationship1 = "MATCH (a:Person {name: 'Alice'}), (b:Person {name: 'Bob'}) CREATE (a)-[:FRIEND]->(b)"
relationship2 = "MATCH (a:Person {name: 'Alice'}), (b:Person {name: 'Charlie'}) CREATE (a)-[:FRIEND]->(b)"
relationship3 = "MATCH (a:Person {name: 'Bob'}), (b:Person {name: 'Charlie'}) CREATE (a)-[:FRIEND]->(b)"

run_query(relationship1)
run_query(relationship2)
run_query(relationship3)
```

We've now created relationships between the nodes we created earlier. Alice is friends with both Bob and Charlie, and Bob is also friends with Charlie.

## Querying the Neo4j database with Python
Now that we have some data in our graph, let's perform some queries to retrieve information from the database.

### Retrieve all Person nodes
```python
query_all_persons = "MATCH (p:Person) RETURN p.name, p.age"

results = run_query(query_all_persons)

for record in results:
    print(f"Name: {record['p.name']}, Age: {record['p.age']}")
```

### Find friends of a specific person

```python 
def get_friends(name):
    query = f"MATCH (p:Person {{name: '{name}'}})-[:FRIEND]->(friend) RETURN friend.name, friend.age"
    results = run_query(query)
    return results

name = "Alice"
friends = get_friends(name)

print(f"Friends of {name}:")
for record in friends:
    print(f"Name: {record['friend.name']}, Age: {record['friend.age']}")
```
### Delete all nodes and relationships
```python 
delete_nodes_and_relationships = "MATCH (n) DETACH DELETE n"
run_query(delete_nodes_and_relationships)
```

This query will delete all nodes and relationships in the database. Be careful when running this, as it will erase all data in your graph!

## Example 
In this example project, we'll create a simple movie recommendation system using Neo4j and Python. We will use a small dataset of movies, actors, and their relationships.

Dataset:

- Actors: Tom Hanks, Meryl Streep, Tom Cruise, Julia Roberts
- Movies: Forrest Gump, The Post, Top Gun, Pretty Woman

Relationships:

- Tom Hanks acted in Forrest Gump
- Tom Hanks acted in The Post
- Meryl Streep acted in The Post
- Tom Cruise acted in Top Gun
- Julia Roberts acted in Pretty Woman


### Create the nodes and relationships in Neo4j
```python
actors = [
    "CREATE (a:Actor {name: 'Tom Hanks'})",
    "CREATE (a:Actor {name: 'Meryl Streep'})",
    "CREATE (a:Actor {name: 'Tom Cruise'})",
    "CREATE (a:Actor {name: 'Julia Roberts'})",
]

movies = [
    "CREATE (m:Movie {title: 'Forrest Gump', year: 1994})",
    "CREATE (m:Movie {title: 'The Post', year: 2017})",
    "CREATE (m:Movie {title: 'Top Gun', year: 1986})",
    "CREATE (m:Movie {title: 'Pretty Woman', year: 1990})",
]

for actor in actors:
    run_query(actor)

for movie in movies:
    run_query(movie)

relationships = [
    "MATCH (a:Actor {name: 'Tom Hanks'}), (m:Movie {title: 'Forrest Gump'}) CREATE (a)-[:ACTED_IN]->(m)",
    "MATCH (a:Actor {name: 'Tom Hanks'}), (m:Movie {title: 'The Post'}) CREATE (a)-[:ACTED_IN]->(m)",
    "MATCH (a:Actor {name: 'Meryl Streep'}), (m:Movie {title: 'The Post'}) CREATE (a)-[:ACTED_IN]->(m)",
    "MATCH (a:Actor {name: 'Tom Cruise'}), (m:Movie {title: 'Top Gun'}) CREATE (a)-[:ACTED_IN]->(m)",
    "MATCH (a:Actor {name: 'Julia Roberts'}), (m:Movie {title: 'Pretty Woman'}) CREATE (a)-[:ACTED_IN]->(m)",
]

for relationship in relationships:
    run_query(relationship)
```

Now that we have our dataset stored in the Neo4j database, let's create a simple movie recommendation function based on the actors a user likes.

### Create the movie recommendation function

```python 
def recommend_movies(liked_actor_name):
    query = f"""
    MATCH (liked_actor:Actor {{name: '{liked_actor_name}'}})-[:ACTED_IN]->(liked_movie:Movie)
    MATCH (other_actor:Actor)-[:ACTED_IN]->(liked_movie)
    MATCH (other_actor)-[:ACTED_IN]->(recommended_movie:Movie)
    WHERE NOT (liked_actor)-[:ACTED_IN]->(recommended_movie)
    RETURN DISTINCT recommended_movie.title AS title, recommended_movie.year AS year
    ORDER BY year DESC
    """

    results = run_query(query)
    return results
```
This function takes the name of an actor that the user likes and recommends movies based on other actors who have acted in the same movies as the liked actor.

#### Get movie recommendations
```python
liked_actor_name = "Tom Hanks"
recommended_movies = recommend_movies(liked_actor_name)

print(f"Movie recommendations based on liking {liked_actor_name}:")
for record in recommended_movies:
    print(f"{record['title']} ({record['year']})")
```

The above code should output the following movie recommendations based on liking Tom Hanks:
```
The Post (2017)
```

In this example, we have created a simple movie recommendation system using a small dataset of actors, movies, and their relationships. This dataset can be easily extended with more nodes and relationships to provide more accurate recommendations. You can also improve the recommendation algorithm by incorporating more factors, such as movie genres or user ratings.

## Wrap-up

Congrats, we have covered how to set up a Neo4j graph database using Docker, connect it with Python, create nodes and relationships, and perform various queries. You should now have a good understanding of how to work with Neo4j and Python to manage graph databases. Remember to explore the official Neo4j Python driver documentation and the Cypher query language reference to expand your knowledge and build more complex graph applications.