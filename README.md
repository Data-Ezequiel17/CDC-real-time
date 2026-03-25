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
\
## How things works
The project uses docker compose to spin up 5 containers. A Postgres container, Kafka container, Debezium container, node.js container, and Kafka-ui container.
The Kafka-ui is just used for the sake of having an interface for Kafka.
The postgres container connects to the Debezium container, which connects to the Kafka container. This is the core of the pipeline. 
Debezium(Producer) captures row-level changes in a PostgreSQL database by reading its Write-Ahead Log (WAL) and streams these changes as event records to Kafka topics.
Once the changes are sent to Kafka topics in the Kafka server a consumer will be subscribed to these topics via the Kafkajs library in node.js server. 
This data is then sent to a dashboard via websocket connection.
\
## Features
-Displays multiple real-time tables showing Create, Read, Update, Delete operations from postgres database
-Able to click on the 'Live Event Chart' and zoom in to a specific CRUD operations happening in real-time.