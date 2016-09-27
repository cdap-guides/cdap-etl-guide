======================================================
Real-time Kafka to TPFS Avro Application Configuration
======================================================

The ``cdap-data-streams`` system artifact can be used to create an application that reads
from a real-time Source and persists it to a Sink. In this example, we will read messages
from Kafka in real time and use a TimePartitionedFileSet Avro batch sink to write the
Kafka messages to HDFS.

In particular, we have a dataset of flight information from a flight sensor (about 5,000
records of airplane flights to destination airports, recording their altitide and velocity
as they pass over the same point) and we would like to know, for each destination airport,
what is the average altitude of the flights heading to that destination.

The file `config.json <config.json>`__ contains a sample application configuration that
you can use to accomplish the above task. Our sample application uses these components:

- The ``cdap-data-streams`` system artifact, since we want to perform in real time
- Kafka source, with a custom kafka.zookeeper configuration
- Window transform, to group the data into batches of 60 seconds of data, every 60 seconds
- GroupByAggregate transform, to calculate the average altitude for each destination airport
- JavaScript transform, to filter out records where the altitude is 500 or greater
- TPFSAvro sink, to write the Kafka events to HBase using the Table dataset

and

- A data set in the form of a CSV text file with flight information: each row being an
  event with a destination airport, an altitude, and a velocity

You can create and start the application by using the CDAP CLI (or you can use the Cask
Hydrator UI for a more visual approach).

**Notes:**

- You can import the `config.json <config.json>`__ into the Cask Hydrator UI: you can
  either import it directly and run it from Hydrator as-is, or you can create an initial
  application as described here using the CLI and then clone it in the UI to develop it
  further.
  
- You need to complete the ``artifact`` version to match the version of CDAP that you are
  using. The version currently in the ``config.json`` is ``3.5.1``.

- You need to complete the ``plugin`` version to match the version of the Hydrator plugins
  that you are using. The version currently in the ``config.json`` is ``1.4.1``.

- This configuration is designed to be used with the CDAP SDK, and uses the SDK's
  Zookeeper and its Kafka ``brokers`` of ``localhost:9092``. If you are running this on a
  different machine or a cluster, change as required.

Creating a Hydrator Application using the CDAP CLI
==================================================
Create a Hydrator application named ``flightIngest`` (replace <version> with your CDAP version)::

  cdap> create app flightIngest cdap-data-streams <version> system RealtimeKafkaToTPFSAvro/config.json
  Successfully created application

  cdap> start spark flightIngest.DataStreamsSparkStreaming
  Successfully started spark 'ETLWorker' of application 'flightIngest' with stored runtime arguments '{}'





To send the data to Kafka, you will need to use an application such as the ``kafka-console-producer``
included in the tools of the Kafka download::

  bin/kafka-console-producer.sh --broker-list localhost:9092 --topic flight < RealtimeKafkaToTPFSAvro/flight_sensor_data.csv


You can verify that the data is being written to the HBase by viewing the contents of the Dataset ``flight_analytics_altitude``, 
by executing this CLI command::

  cdap> execute 'select * from dataset_flight_analytics_altitude'

You have now successfully created an Adapter that reads from Kafka and writes to HDFS.

You can stop and delete the application using the CDAP CLI::

  cdap> stop spark flightIngest.DataStreamsSparkStreaming
  Successfully stopped spak 'ETLWorker' of application 'flightIngest'

  cdap> delete app flightIngest
  Successfully deleted application 'flightIngest'

Share and Discuss!
==================

Have a question? Discuss at the `CDAP User Mailing List <https://groups.google.com/forum/#!forum/cdap-user>`__.

License
=======

Copyright © 2015-2016 Cask Data, Inc.

Licensed under the Apache License, Version 2.0 (the "License"); you may
not use this file except in compliance with the License. You may obtain
a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

