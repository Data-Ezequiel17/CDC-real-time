# CDC-real-time
A containerized Change Data Capture pipeline that displays in real-time all changes occurring in a Postgres database using Debezium, Kafka and Node.js websocket server
<img width="2240" height="1200" alt="image" src="https://github.com/user-attachments/assets/47234c60-1f27-46f2-bfa8-e5de5abbc093" />

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
The project uses docker compose to spin up 6 containers: A Postgres container, Kafka container, Debezium container, cdc-consumer container, Node.js container, and a Provectus Kafka-ui container. \
\
The Kafka-ui is just used for the sake of having an interface for Kafka. The cdc-consumer container hosts a python consumer via the consumer.py script. This is one of the two consumers subcribed to the kafka topics via confluent_kafka library. The other consumer is the Node.js websocket server which sends the data to the dashboard. The cdc-consumer basically shows the same thing the dashboard does, but in the logs(in docker desktop). \
\
Debezium captures row-level changes in a PostgreSQL database by reading its Write-Ahead Log (WAL) then serializes this data into json and streams the changes as event records to Kafka connect, which streams this data to Kafka topics.
Once the changes are sent to Kafka topics in the Kafka server a consumer(node.js server) will be subscribed to these topics and poll the data in real-time via the Kafkajs library. 
This data is then sent to the dashboard on your browser via websocket connection.

https://github.com/user-attachments/assets/b0e5954a-0834-43d9-b780-65b2f22aba73

## How to run
**prequalifications:**
+ have docker desktop installed and running

**STEP 1** - create a python virtual environment in your IDE by running these two commands in terminal: 
```bash 
python -m venv venv
```
```bash 
venv\Scripts\activate
```

**STEP 2** - once virtual environment is running, run pip install command to install requirements for the generate-data.py script:
```bash
 pip install -r scripts/requirements.txt 
```

**STEP 3** - once that is done, run docker compose up:
```bash 
docker compose up -d
```

**STEP 4** - When all containers are up and running(takes few mins) go to debezium container logs in docker desktop and scroll to bottom till you see ```'finished starting connectors and tasks'```.
Then run the command below. If you have a mac, run the first. Else run the second command for windows. This command takes the configuration file (postgres-connector.json) and sends it to your local Kafka Connect service to start a Debezium process that captures changes from a PostgreSQL database. 
The postgres-connector.json config file tells Kafka Connect how to connect to your PostgreSQL database. 

+ for mac:

```bash
curl -X POST http://localhost:8083/connectors -H "Accept: application/json" -H "Content-Type: application/json" -d @"debezium/postgres-connector.json"
```
+ for windows:
```bash
Invoke-WebRequest -Uri http://localhost:8083/connectors `                        
                   -Method Post `                                                                                                            
                   -Headers @{                                                                                                               
                       "Accept" = "application/json"
                      "Content-Type" = "application/json"
                  } `
                   -InFile "debezium/postgres-connector.json"
```
After running one of the commands from above go to http://localhost:8083/connectors and check to see that the connector file is there:```["postgres-cdc-connector"]```

to see dashboard go to http://localhost:3000/ \
to see kafka-ui go to http://localhost:8080/

**STEP 5** - finally run the python script to generate data: 

```bash
python scripts/generate-data.py --interval 2
```
you can modify the interval to anything you want. This number is the interval at which data will be generated. So '--interval 2' means data will be generated every 2 seconds. 
'--interval 0.5' means every 0.5 secs data will be generated, etc..

\
\
**TO TERMINATE PROGRAMS** 

+ to end the generate_data.py script: \
```Ctrl+C```

+ to shut down docker containers:
```bash
docker compose down -v
```
+ to end python virtual environment:
```bash
deactivate
```

## Features
+ Displays multiple real-time tables showing Create, Read, Update, Delete operations from postgres database.
+ Able to click on the 'Live Event Chart' and zoom into a specific CRUD operations happening in real-time.
+ Hover over certain charts and get more details.

## Project files description
consumer folder
+ **consumer.py**  - python script containing consumer logic with help of confluent_kafka python library.
+ **Dockerfile** (consumer)  - blueprint for the cdc-consumer docker image.
+ **requirements.txt**  - specify the external packages and libraries required for the cdc-consumer image to run.

dashboard folder
+ **index.html** - dashboard code. where node.js server sends the cdc data.
+ **Dockerfile** (dashboard) - blueprint for the dashboard docker image.
+ **package.json** - heart of any Node.js project, serving as a manifest that records essential metadata, dependencies, and script commands.
+ **server.js** - server-side code. Responsible for creating and starting the node.js websocket server.

debezium folder
+ **postgres-connector.json**  - config file to register/define the behavior of Debezium PostgreSQL connector within Kafka Connect framework.

postgres folder
+ **init.sql**  - SQL script used for database initialization, which involves setting up the initial database structure, tables, and data.
+ **pg_hba.conf**  - primary configuration file for controlling client authentication in PostgreSQL.
+ **postgresql.conf**  - primary config file for PostgreSQL database server, used to control parameters like memory usage, logging, and connection settings.

scripts folder
+ **generate-data.py**  - python script that generates fake, but realistic data to mimick a live production database environment.
+ **requirements.txt**  - specify the external packages and libraries required for the python virtual env.
+ **docker_compose.yml**  - YAML configuration file used by Docker Compose to define and manage multi-container Docker applications.










