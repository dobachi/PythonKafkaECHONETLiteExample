# PythonKafkaECHONETLiteExample

This is an example to use Apache Kafka to store ECHONET Lite data.

## Preparements

* Install Apache Kafka and configure the cluster
* Clone this repository

## Note

* This example is tested using Python 3.7.10 via pyenv.
  * sqlite-devel and libffi-devel should be installed before compiling Python.

## Basic Example of Kafka Client

```shell
$ jupyter-lab
```

* BasicExampleOfKafka.ipynb

## Basic Example of Kafka clinet to write data of ECHONET Lite

```shell
$ jupyter-lab
```

* BasicExampleOfKafkaECHONETLite.ipynb

## Basic Example to write data into Delta Lake

```shell
$ jupyter-lab
```

* IngestDataToDeltaLake.ipynb

## References

* [Confluent Python Client]

[Confluent Python Client]: https://docs.confluent.io/clients-confluent-kafka-python/current/overview.html