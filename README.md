# Kafka Connect Exasol Example

This is an example repository to show how to setup [Kafka
Connect][kafka-connect] Exasol Connector.

We are going to use [Kafka JDBC Connector][kafka-jdbc] with Exasol dialect which
is currently in early stages of development. Therefore, please bear in mind that
this is a work in progress at the moment.

## Table of Contents

* [Quick Start](#quick-start)
* [Gotchas](#gotchas)
* [License](#license)

## Quick Start

For testing we are going to use [docker][docker] and
[docker-compose][docker-compose]. Please set them up accordingly on your local
machine. **Additionally, if you are using non Linux machine, please obtain the
ip address for docker or docker-machine**. For example, in MacOS, with the
following command:

```bash
docker-machine ip
```

For the rest of documentation, when we refer to `localhost`, substitute it with
ip address resulted from above command.

We need to open several terminals for dockerized testing.

* In terminal one, clone the repository and start all the services:

```bash
git clone https://github.com/exasol/kafka-exasol-example.git

cd kafka-exasol-example

docker-compose up
```

* In terminal two, create a schema and add some records to Exasol DB.  This
  creates a `country` table inside `country_schema` and inserts couple of
  records into it. **This step should happen before Kafka connector
  configurations setup because Kafka immediately starts to look for the Exasol
  tables.**

```bash
docker exec -it exasol-db /bin/bash

cd /test/ && ./create_tables.sh
```

* Once the table is created, open another terminal and upload connecter
  configuration file to kafka connect:

```bash
# Create and add a new Kafka Connect Source
curl -X POST \
     -H "Content-Type: application/json" \
     --data @exasol-source.json localhost:8083/connectors

# You can see all available connectors with the command:
curl localhost:8083/connectors/

# Similarly, you can see the status of a connector with the command:
curl localhost:8083/connectors/exasol-source/status
```

* Lastly, on terminal four, let us consume some data from Kafka. For the purpose
  of this example we are going to use kafka console consumer:

```bash
docker exec -it kafka02 /bin/bash

# List available Kafka topics, we should see the 'EXASOL_COUNTRY' listed.
kafka-topics --list --zookeeper zookeeper.internal:2181

# Start kafka console consumer
kafka-console-consumer \
    --bootstrap-server kafka01.internal:9092 \
    --from-beginning \
    --topic EXASOL_COUNTRY
```

* You should see two records inserted from other terminal. Similarly, if you
  insert new records into `country` table in Exasol, they should be listed on
  kafka consumer console.

## Gotchas

There are several tips and tricks to consider when setting up the Kafka Exasol
connector.

* The timestamp and incrementing column names should be in upper case, for
  example, `"timestamp.column.name": "UPDATED_AT"`. This is due to fact that
  Exasol makes all fields upper case and Kafka connector is case sensitive.

* The `tasks.max` should be more than the number of tables in production
  systems. That is in jdbc connectors each table is sourced per partition then
  handled by single task.

## License

[BSD 3-Clause "New" or "Revised" License](LICENSE)

[kafka-connect]: http://kafka.apache.org/documentation.html#connect
[kafka-jdbc]: https://github.com/confluentinc/kafka-connect-jdbc
[docker]: https://www.docker.com/
[docker-compose]: https://docs.docker.com/compose/
