Realtime Stream To Impala Adapter Configuration
===============================================

The ETL Realtime Template can be used to create an Adapter that reads from a Realtime Source and persists it to a Sink.
In this example, we will read events from a Stream in realtime and use a TPFS Sink to make the data queryable by Impala.

The config.json contains a sample Adapter configuration that you can use to accomplish the above task. Our sample Adapter uses the following components:

- etlRealtime Application Template, since we want to perform ETL in realtime
- Stream source, configured to read from the myStream Stream
- TPFSAvro sink, configured to write to the converted_stream Dataset

You can create and start the Adapter by using the CDAP CLI (or you can use the UI for a more visual approach)::

  cdap> create adapter trades_conversion RealtimeStreamToImpala/config.json
  Successfully created adapter 'trades_conversion'

  cdap> start adapter trades_conversion
  Successfully started adapter 'trades_conversion'

This will create a schedule that will run the Adapter every ten minutes. 
The next time the Adapter runs, it will spawn a MapReduce job that reads all events added
in the past ten minutes, writes each event to Avro encoded files, and registers a new
partition in the Hive Metastore. We can then query the contents using Impala. On a
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

Now that you have data in CDAP and are able to explore your data, you can use CDAP's many
useful and powerful services, such as the ability to dynamically scale processing units,
distributed transactions, and service discovery, to write Applications that meet your
business needs.

You can stop and delete the Adapter using the CDAP CLI::

  cdap> stop adapter trades_conversion
  Successfully stopped adapter 'trades_conversion'

  cdap> delete adapter trades_conversion
  Successfully deleted adapter 'trades_conversion'

