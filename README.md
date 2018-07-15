# Kafka Connect Exasol Example

This is an example repository to show how to setup Kafka Exasol Connector.

We are going to use Kafka JDBC Connector with Exasol dialect which is currently in early stages.
Therefore, please bear in mind that this is work in progress currently.

## QuickStart

First let us clone the repository and start the services using docker and docker-compose:

```bash
$ git clone https://github.com/exasol/kafka-exasol-example.git 

$ cd kafka-exasol-example

$ docker-compose up
```

Now let's create a Exasol schema and add some records.  This should happen before Kafka connector
configurations setup because kafka immediately starts to look for the Exasol tables.

```bash
$ docker exec -it exasol-db /bin/bash

$ cd /test/ && ./create_tables.sh 
```

Then open another terminal and upload connecter configuration file to kafka connect:

```bash
$ curl -X PUT exasol-source.json localhost:8083/connectors

# You can see all available connectors with command:
# 
# Similarly, you can list all available connectors with command:
#
```

Now let's consume the data from Kafka. For the purpose of this example we are goint to use kafka
console consumer:

```bash

```
