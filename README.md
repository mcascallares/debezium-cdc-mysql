# Overview

## Starting the environment

```
docker-compose up -d
```


## Listing connectors

```
docker exec -it connect curl -X GET http://localhost:8083/connectors
```


## Creating the connector using the REST API

```
docker exec -it connect curl -X PUT \
    -H "Content-Type: application/json" \
    -d '{
        "connector.class": "io.debezium.connector.mysql.MySqlConnector",
        "database.hostname": "mysql",
        "database.port": "3306",
        "database.user": "root",
        "database.password": "root",
        "database.server.id": "1",
        "database.server.name": "mysql",
        "database.include.list": "inventory",
        "database.history.kafka.bootstrap.servers": "kafka:9092",
        "database.history.kafka.topic": "inventory.products",
        "include.schema.changes": "true"
    }' http://localhost:8083/connectors/inventory-connector/config
```


## Accessing Mysql Database and create sample data

```
docker exec -it mysql mysql --user=root --password=root

CREATE DATABASE inventory;
USE inventory;
CREATE TABLE products (id INT, name VARCHAR(20));
INSERT INTO products (id, name) values (1, 'iPhone X');
INSERT INTO products (id, name) values (2, 'MacBook');
```

## Listing Kafka topics

```
docker exec -it kafka kafka-topics --bootstrap-server localhost:9092 --list
```


## Consuming DDL messages from Kafka

```
docker exec -it kafka kafka-console-consumer --bootstrap-server localhost:9092 --topic inventory.products --from-beginning
```


## Consuming data messages from Kafka

```
docker exec -it kafka kafka-console-consumer --bootstrap-server localhost:9092 --topic mysql.inventory.products --from-beginning
```
