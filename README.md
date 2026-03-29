# CDC-real-time
A containerized Change Data Capture pipeline that displays in real-time all changes occurring in a Postgres database using Debezium, Kafka and Node.js websocket server

## Technologies 
+ Python 
+ Javascript/HTML
+ Node.js 
+ Kafka 
+ Docker 
+ Debezium 
+ Postgres


## About the project
This project is a production-ready real-time CDC(Capture Data Change) pipeline. It uses Debezium to capture any changes (Inserts, Updates, Reads, Deletes) in a 
Postgres database and sends this info to Kafka topics in a Kafka server. \
\
This Postgres database mimicks a typical E-commerce database. 
It has a customers, products, orders, inventory, and order_items tables. Any changes to these tables will be picked up and streamed in real-time to a
Node.js websocket server dashboard which can be accessed on a browser. \
\
A Python script using the Faker library is used to create fake, but realistic looking data to 
simulate the CRUD operations that happnen on an e-commerce database everyday. This includes Inserts, Updates, Reads, and Deletes. 
When these changes are made on the tables they are broadcasted on the dashboard instantly.

## How things works
The project uses docker compose to spin up 5 containers. A Postgres container, Kafka container, Debezium container, node.js container, and Kafka-ui container.
The Kafka-ui is just used for the sake of having an interface for Kafka. \
\
The postgres container connects to the Debezium container, which connects to the Kafka container. This is the core of the pipeline. \
\
Debezium captures row-level changes in a PostgreSQL database by reading its Write-Ahead Log (WAL) then serializes this data into json and streams the changes as event records to Kafka connect, which streams this data to Kafka topics.
Once the changes are sent to Kafka topics in the Kafka server a consumer(node.js server) will be subscribed to these topics and poll the data in real-time via the Kafkajs library. 
This data is then sent to the dashboard on your browser via websocket connection.

## Features
+ Displays multiple real-time tables showing Create, Read, Update, Delete operations from postgres database.
+ Able to click on the 'Live Event Chart' and zoom in to a specific CRUD operations happening in real-time.
+ Hover over certain charts and get more details.

## Project files description
consumer folder
+ **consumer.py**  - python script containing consumer logic with help of confluent_kafa python library
+ **dockerfile** (consumer)  - blueprint for the cdc-consumer docker image
+ **requirements.txt**  - specify the external packages and libraries required for the cdc-consumer image to run

debezium folder
+ **postgres-connector.json**  - configuration file used to register and define the behavior of the Debezium PostgreSQL connector within the Kafka Connect framework.

postgres folder
+ **init.sql**  - SQL script used for database initialization, which involves setting up the initial database structure, tables, and data
+ **pg_hba.conf**  - primary configuration file for controlling client authentication in PostgreSQL
+ **postgresql.conf**  - primary configuration file for the PostgreSQL database server, used to control various runtime parameters like memory usage, logging, and connection settings

scripts folder
+ **generate-data.py**  - python script that generates fake, but realistic data to mimick a live production database environment
+ **requirements.txt**  - specify the external packages and libraries required for the python virtual env

+ **docker_compose.yml**  - YAML configuration file used by Docker Compose to define and manage multi-container Docker applications
