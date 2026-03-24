# CDC-real-time
A containerized Change Data Capture pipeline that displays in real-time all changes occurring in a Postgres database using Debezium, Kafka and Node.js websocket server

## Technologies 
Python \
Javascript/HTML \
Node.js \
Kafka \
Docker \
Debezium \
Postgres


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
