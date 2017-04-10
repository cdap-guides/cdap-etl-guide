======================================================
Real-time Kafka to TPFS Avro Application Configuration
======================================================

The ``cdap-data-streams`` system artifact can be used to create an application that reads
from a real-time source and persists it to a sink. In this example, we will read messages
from Kafka in real time and use a TimePartitionedFileSet Avro batch sink to write the
Kafka messages to HDFS.

For this example we have a dataset of flight information from a flight sensor (about 5,000
records of airplane flights to destination airports, recording their altitide and velocity
as they pass over the same point). We would like to know, for each destination airport,
the average altitude of all flights heading to that destination.

The file `config.json <config.json>`__ contains a sample application configuration that
you can use to accomplish this task. Our sample application uses these components:

- ``cdap-data-streams`` system artifact, since we want to perform in real time
- Kafka source, with a custom ``kafka.zookeeper`` configuration
- Window transform, to group the data into batches of 60 seconds of data, every 60 seconds
- GroupByAggregate transform, to calculate the average altitude for each destination airport
- JavaScript transform, to filter out records where the altitude is 500 or greater
- TPFSAvro sink, to write the results to HBase using the Table dataset

and

- Data in the form of a CSV text file with flight information: each row an event with a
  destination airport, an altitude, and a velocity, in
  ``RealtimeKafkaToTPFSAvro/flight_sensor_data.csv``

**Notes:**

- You can create and start the application by using the CDAP CLI (or you can use the CDAP
  UI for a more visual approach).

- You can import the `config.json <config.json>`__ into the CDAP UI: you can
  either import it directly and run it from CDAP as-is, or you can create an initial
  application as described here using the CLI and then clone it in the UI to develop it
  further.

- You need to complete the ``artifact`` version to match the version of CDAP that you are
  using. The version currently in the ``config.json`` is ``3.5.1``.

- You need to complete the ``plugin`` version to match the version of the CDAP plugins
  that you are using. The version currently in the ``config.json`` is ``1.4.1``.

- This configuration is designed to be used with the CDAP SDK, and uses the SDK's
  Zookeeper and its Kafka ``brokers`` of ``localhost:9092``. If you are running this on a
  different machine or a cluster, change these as required.


Starting CDAP
=============
If you haven't already, start your CDAP installation::

  $ cdap sdk start


Creating a CDAP Pipeline using the CDAP CLI
===========================================
From within the CDAP CLI, create a pipeline named ``flightIngest``, replacing
<version> with your CDAP version::

  cdap> create app flightIngest cdap-data-streams <version> system RealtimeKafkaToTPFSAvro/config.json
  Successfully created application

  cdap> start spark flightIngest.DataStreamsSparkStreaming
  Successfully started Spark program 'DataStreamsSparkStreaming' of application 'flightIngest' with stored runtime arguments '{}'

While the real-time application is running, you can then send the dataset of flight information.

To setup and send the data to Kafka, you will need applications such as the
``kafka-console-producer`` included in the tools of the Kafka download. The version of
Kafka needs to match the version being used in CDAP. For example, you can obtain version
2.10 of the Kafka tools from apache.org and untar them::

  https://www.apache.org/dyn/closer.cgi?path=/kafka/0.8.2.0/kafka_2.10-0.8.2.0.tgz

  $ tar -xzf kafka_2.10-0.8.2.0.tgz
  $ cd kafka_2.10-0.8.2.0

Now, while the CDAP application is running, send it the flight sensor data::

  $ bin/kafka-console-producer.sh --broker-list localhost:9092 --topic flight < RealtimeKafkaToTPFSAvro/flight_sensor_data.csv

Note: As necessary, modify paths, hostnames, and ports depending on your particular installation.

The data will be uploaded into Kafka, run through the pipeline, and the results written to HBase.

You can verify that the results have been written to HBase by viewing the contents of the
dataset ``flight_analytics_altitude`` by executing this CLI command::

  cdap> execute 'select * from dataset_flight_analytics_altitude'

  +==================================================================================================================================================+
  | dataset_flight_ana | dataset_flight_ana | dataset_flight_ana | dataset_flight_ana | dataset_flight_ana | dataset_flight_ana | dataset_flight_ana |
  | lytics_altitude.de | lytics_altitude.al | lytics_altitude.ye | lytics_altitude.mo | lytics_altitude.da | lytics_altitude.ho | lytics_altitude.mi |
  | stination_airport: | titude: FLOAT      | ar: INT            | nth: INT           | y: INT             | ur: INT            | nute: INT          |
  |  STRING            |                    |                    |                    |                    |                    |                    |
  +==================================================================================================================================================+
  | SFO                | 263.9689636230469  | 2016               | 9                  | 16                 | 17                 | 18                 |
  | SAN                | 356.1403503417969  | 2016               | 9                  | 16                 | 17                 | 18                 |
  | BUR                | 300.0              | 2016               | 9                  | 16                 | 17                 | 18                 |
  | LGB                | 300.0              | 2016               | 9                  | 16                 | 17                 | 18                 |
  +==================================================================================================================================================+
  Fetched 4 rows

You have now successfully created an application that reads from Kafka and writes to HDFS.

Stopping and Deleting
---------------------
You can stop and delete the application using the CDAP CLI::

  cdap> stop spark flightIngest.DataStreamsSparkStreaming
  Successfully stopped Spark program 'DataStreamsSparkStreaming' of application 'flightIngest'

  cdap> delete app flightIngest
  Successfully deleted application 'flightIngest'


Share and Discuss!
==================

Have a question? Discuss at the `CDAP User Mailing List <https://groups.google.com/forum/#!forum/cdap-user>`__.

License
=======

Copyright © 2016-2017 Cask Data, Inc.

Licensed under the Apache License, Version 2.0 (the "License"); you may
not use this file except in compliance with the License. You may obtain
a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

