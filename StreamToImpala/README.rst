=====================================================
Batch CDAP Stream to Impala Application Configuration
=====================================================

The built-in ``cdap-data-pipeline`` system artifact can be used to create a data pipeline
that reads from a Batch source and persists it to a sink. In this example, we will read
events from a stream in batch and use the TPFSAvro sink to make the data queryable by Impala.

The file `config.json <config.json>`__ contains a sample application configuration that
you can use to accomplish the above task. Our sample application uses these components:

- The ``cdap-data-pipeline`` system artifact, since we want to perform the pipeline in batch
- Stream source, configured to read from the *trades* stream
- TPFSAvro sink, configured to write to the *trades_converted* dataset

You can create and start the application by using the CDAP CLI (or you can use the Cask
Hydrator UI for a more visual approach).

**Notes:**
  
- If you want to import the ``config.json`` into the Cask Hydrator UI, you will need to
  modify it to include an ``artifact`` property describing the system artifact being used.
  You can create an initial application as described here using the CLI and then clone it
  in the UI to develop it further.


Creating a Hydrator Application using the CDAP CLI
==================================================
First, load some trade events to be processed by your application::

  cdap> create stream trades
  Successfully created stream with ID 'trades'

  cdap> send stream trades 'NFLX|50|441.07'
  Successfully sent stream event to stream 'trades'

  cdap> send stream trades 'AAPL|100|118.63'
  Successfully sent stream event to stream 'trades'

  cdap> send stream trades 'GOOG|10|528.48'
  Successfully sent stream event to stream 'trades'

Then you can create and start the application by using the CDAP CLI (replace <version>
with your CDAP version)::

  cdap> create app trades_conversion cdap-data-pipeline <version> system StreamToImpala/config.json
  Successfully created application

  cdap> start workflow trades_conversion.DataPipelineWorkflow
  Successfully started workflow 'DataPipelineWorkflow' of application 'trades_conversion' with stored runtime arguments '{}'

This will run the workflow, which will spawn a MapReduce job that reads all events added
in the past ten minutes, writes each event to Avro-encoded files, and registers a new
partition in the Hive Metastore. You can also schedule the workflow to run periodically::

  cdap> resume schedule trades_conversion.dataPipelineSchedule
  Successfully resumed schedule 'dataPipelineSchedule' in app 'trades_conversion'

After the workflow has run, we can query the contents using Impala. On a
cluster, use the Impala shell to connect to Impala::

  $ impala-shell -i <impala-host>
  > invalidate metadata
  > select ticker, sum(price * num) as volume from dataset_trades_converted group by ticker order by volume desc
  +--------+----------+
  | ticker | volume   |
  +--------+----------+
  | NFLX   | 22053.5  |
  | AAPL   | 11863.0  |
  | GOOG   | 5284.8   |
  +--------+----------+
  Fetched 3 row(s) in 1.03s

Since we are using Impala, no MapReduce jobs are launched, and the query comes back in
about one second.

Suspending and Deleting
-----------------------
You can suspend and delete the application using the CDAP CLI::

  cdap> suspend schedule trades_conversion.dataPipelineSchedule
  Successfully suspended schedule 'dataPipelineSchedule' in app 'trades_conversion'

  cdap> delete app trades_conversion
  Successfully deleted application 'trades_conversion'


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

