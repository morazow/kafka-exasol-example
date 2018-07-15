# Kafka Connect Exasol Example

This is an example repository to show how to setup Kafka Exasol Connector.

We are going to use Kafka JDBC Connector with Exasol dialect which is currently in early stages.
Therefore, please bear in mind that this is a work in progress at the moment.

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

This creates a `country` table inside `country_schema` and insert couple records into it.

Once the table is created, then open another terminal and upload connecter configuration file to
kafka connect:

```bash
$ curl -X POST -H "Content-Type: application/json" --data @exasol-source.json localhost:8083/connectors

# You can see all available connectors with the command:
# $ curl localhost:8083/connectors/
# Similarly, you can see the status of a connector with the command:
# $ curl localhost:8083/connectors/exasol-source/status
```

Now let's consume the data from Kafka. For the purpose of this example we are going to use kafka
console consumer:

```bash
$ docker exec -it kafka02 /bin/bash

# List available Kafka topics, we should our 'EXASOL_COUNTRY' listed.
$ kafka-topics --list --zookeeper zookeeper.internal-service:2181

# Start kafka console consumer
$ kafka-console-consumer --bootstrap-server kafka01.internal-service:9092 --from-beginning --topic EXASOL_COUNTRY
```

You should see two records we inserted on other terminal. Similarly, if you insert new records
into `country` table in Exasol, they should be listed on kafka console consumer.
