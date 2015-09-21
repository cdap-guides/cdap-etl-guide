=====================================================
Batch CDAP Stream To Impala Application Configuration
=====================================================

The ``cdap-etl-batch`` system artifact can be used to create an Application that reads from a 
Batch Source and persists it to a Sink.
In this example, we will read events from a Stream in batch and use a TPFS Sink to make the data queryable by Impala.

The ``config.json`` contains a sample Application configuration that you can use to accomplish the above task. 
Our sample Application uses the following components:

- The ``cdap-etl-batch`` system artifact, since we want to perform ETL in batch
- Stream source, configured to read from the *trades* Stream
- TPFSAvro sink, configured to write to the *trades_converted* Dataset

First, load some trade events to be processed by our Application::

  cdap> create stream trades
  Successfully created stream 'trades'

  cdap> send stream trades 'NFLX|50|441.07'
  Successfully sent stream event to stream 'trades'

  cdap> send stream trades 'AAPL|100|118.63'
  Successfully sent stream event to stream 'trades'

  cdap> send stream trades 'GOOG|10|528.48'
  Successfully sent stream event to stream 'trades'

Then you can create and start the Application by using the CDAP CLI (or you can use the UI for a more visual approach)::

  cdap> create app trades_conversion cdap-etl-batch <version> system StreamToImpala/config.json
  Successfully created application

  cdap> start workflow trades_conversion.ETLWorkflow
  Successfully started workflow 'ETLWorkflow' of application 'trades_conversion' with stored runtime arguments '{}'

This will run the workflow, which will spawn a MapReduce job that reads all events added
in the past ten minutes, writes each event to Avro-encoded files, and registers a new
partition in the Hive Metastore. You can also schedule the workflow to run periodically::

  cdap> resume schedule trades_conversion.etlWorkflow 
  Successfully resumed schedule 'etlWorkflow' in app 'trades_conversion'

After the workflow has run, we can query the contents using Impala. On a
cluster, use the Impala shell to connect to Impala::

  $ impala-shell -i <impala-host>
  > invalidate metadata
  > select ticker, sum(price * trades) / 1000000 as millions from dataset_trades_converted group by ticker order by millions desc
  +--------+-------------------+
  | ticker | millions          |
  +--------+-------------------+
  | AAPL   | 3121.88477111439  |
  | NFLX   | 866.0568582408006 |
  | GOOG   | 469.0081187983999 |
  +--------+-------------------+
  Fetched 3 row(s) in 1.03s

Since we are using Impala, no MapReduce jobs are launched, and the query comes back in
about one second.

You can delete the Application using the CDAP CLI::

  cdap> delete app trades_conversion
  Successfully deleted application 'trades_conversion'
