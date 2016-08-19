==================================================
Real-time Kafka to HBase Application Configuration
==================================================

The ``cdap-etl-realtime`` system artifact can be used to create an Application that reads
from a real-time Source and persists it to a Sink. In this example, we will read messages
from Kafka in real time and use a TableSink to write the Kafka messages to HBase.

The file `config.json <config.json>`__ contains a sample Application configuration that
you can use to accomplish the above task. Our sample Application uses these components:

- The ``cdap-etl-realtime`` system artifact, since we want to perform ETL in real time
- Kafka source, with our custom kafka.zookeeper configuration
- Table sink, to write the Kafka events to HBase using the Table dataset

You can create and start the Application by using the CDAP CLI (or you can use the Cask
Hydrator UI for a more visual approach).

**Notes:**

- You need to fill in the following configurations in a file such as the `config.json
  <config.json>`__ before creating the Application.
  
- If you want to import the ``config.json`` into the Cask Hydrator UI, you will need to
  modify it to include an ``artifact`` property describing the system artifact being used.
  You can create an initial application as described here using the CLI and then clone it
  in the UI to develop it further.

- You need to fill in the ``kafka.zookeeper`` configuration in ``config.json`` before
  creating the Application. This is the connection string to your Kafka server in ZooKeeper.

Creating an ETL Application using CDAP CLI
==========================================
Create an ETL Application named ``kafkaIngest`` (replace <version> with your CDAP version)::

  cdap> create app kafkaIngest cdap-etl-realtime <version> system RealtimeKafkaToHBase/config.json
  Successfully created application

  cdap> start worker kafkaIngest.ETLWorker
  Successfully started worker 'ETLWorker' of application 'kafkaIngest' with stored runtime arguments '{}'

You can verify that the data is being written to the HBase by viewing the contents of the Dataset metrics, 
by executing the following CLI command::

  cdap> execute 'select * from dataset_metrics'

You have now successfully created an Adapter that reads from Kafka and writes to HBase.

You can stop and delete the Application using the CDAP CLI.

::

  cdap> stop worker kafkaIngest.ETLWorker
  Successfully stopped worker 'ETLWorker' of application 'kafkaIngest'

  cdap> delete app kafkaIngest
  Successfully deleted application 'kafkaIngest'

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

