Realtime Kafka To HBase Application Configuration
=================================================

The cdap-etl-realtime system artifact can be used to create an Application that reads from a Realtime Source and persists it to a Sink.
In this example, we will read messages from Kafka in realtime and use a TableSink to write the Kafka messages to HBase.

The config.json contains a sample Application configuration that you can use to accomplish the above task. Our sample Application uses the following components:

- cdap-etl-realtime system artifact, since we want to perform ETL in realtime
- Kafka source, with our custom kafka.zookeeper configuration
- Table sink, to write the Kafka events to HBase using the Table dataset

You can create and start the Application by using the CDAP CLI (or you can use the UI for a more visual approach).

Note: You need to fill in the ``kafka.zookeeper`` configuration in config.json before creating the Application.
This is the connection string to your Kafka server in ZooKeeper.

::

  cdap> create app kafkaIngest cdap-etl-realtime <version> system RealtimeKafkaToHBase/config.json
  Successfully created application

  cdap> start worker kafkaIngest.ETLWorker
  Successfully started worker 'ETLWorker' of application 'kafkaIngest' with stored runtime arguments '{}'

You can verify that the data is being written to the HBase by viewing the contents of the Dataset, metrics, by executing the following CLI command:

::

  cdap> execute 'select * from dataset_metrics'

You have now successfully created an Adapter that reads from Kafka and writes to HBase.

You can stop and delete the Application using the CDAP CLI.

::

  cdap> stop worker kafkaIngest.ETLWorker
  Successfully stopped worker 'ETLWorker' of application 'kafkaIngest'

  cdap> delete app kafkaIngest
  Successfully deleted application 'kafkaIngest'

