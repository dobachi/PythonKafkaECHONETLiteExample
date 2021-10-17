# PythonKafkaECHONETLiteExample

This is an example to use Apache Kafka to store ECHONET Lite data.

## Preparements

* Install Apache Kafka and configure the cluster
* Clone this repository

## Note

* This example is tested using Python 3.7.10 via pyenv.
  * sqlite-devel and libffi-devel should be installed before compiling Python.

## Basic Example of Kafka Client

BasicExampleOfKafka.ipynb

## Basic Example of Kafka clinet to write data of ECHONET Lite

BasicExampleOfKafkaECHONETLite.ipynb

## Basic Example to write data into Delta Lake

IngestDataToDeltaLake.ipynb

## Basic Example of Delta Sharing client

BasicExampleDeltaShring.ipynb

## Helper Notebook to Transform Power Grid Data

GenerateDummyPowerGridData.ipynb

This notebook generates a dummy data of the power grid data referencing the real data.
You can obtain the real data from power grid's web site.

## Dummy Data of Power Grid

`data/power_grid_dummydata` is a dummy data of power grid data.
The format is Delta Lake.
This is not real data but dummy data and generated referencing to the power grid data.

## References

* [Confluent Python Client]

[Confluent Python Client]: https://docs.confluent.io/clients-confluent-kafka-python/current/overview.html