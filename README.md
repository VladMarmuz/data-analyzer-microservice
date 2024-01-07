# Data analyser microservice

This is data analyser microservice

This application receives data
from [Data generator service](https://github.com/VladMarmuz/data-generator-microservice)
with Apache Kafka.

Next, data is processed
by [Data store service](https://github.com/VladMarmuz/data-store-microservice).

### Usage

To start an application you need to pass variables to `.env` file.

You can find Docker compose file in `docker/docker-compose.yaml`.

Application is running on port `8082`.

All insignificant features (checkstyle, build check, dto validation) are not
presented.

#### Example:

```agsl
HOST=localhost:5437
POSTGRES_DB=sensor_data
POSTGRES_USERNAME=postgres
POSTGRES_PASSWORD=postgres
KAFKA_BOOTSTRAP_SERVERS=localhost:9092
KAFKA_SUBSCRIBED_TOPICS=data-temperature,data-power,data-voltage
```

Just after startup application will try to connect to Apache Kafka and begin to
listen topics from `KAFKA_SUBSCRIBED_TOPICS`.

### Docker

You can run all course applications via `docker-compose.yaml` from `docker`
folder.

It contains all needed configs.

**NOTE**: after Debezium connect is started, apply source config manually.

```shell
cd /on-startup/

curl -i -X POST -H "Accept:application/json" -H \
"Content-Type:application/json"  http://localhost:8083/connectors/ -d \
@postgres-connector.json
```

Note that all services must be in the same network to communicate with each
other.

Debezium needs different group id than Kafka uses, so default values from `.env`
are 1 and 2.

Debezium is configured to push messages to `data` topic due to routing in
configuration.
