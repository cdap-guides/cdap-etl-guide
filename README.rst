===========================================================
Creating ETL Pipelines using CDAP ETL Application Templates
===========================================================

Using CDAP ETL Application Templates, you can create ETL Pipelines (ETL Adapters) with just a JSON configuration file. 
CDAP ships with a set of built-in Sources, Sinks and Transforms [http://docs.cask.co/cdap/current/en/application-templates/index.html] 
which can be used to create Batch ETL and Realtime ETL Adapters right out of the box.

Note: If you want to create your own Source and Sink, you can find more instructions on how to do that here : 
[http://docs.cask.co/cdap/current/en/application-templates/etl/custom.html]

What You Will Create
====================

[Realtime Twitter To HBase](/RealtimeTwitterToHBase/)
---------------------------

In this Adapter, we will read Tweets from Twitter in Realtime and write to HBase Table.


[Realtime Kafka To HBase](/RealtimeKafkaToHBase/)
-------------------------

In this Adapter, we will fetch messages from Kafka in Realtime and write to HBase Table.


[Batch CDAP Stream To Impala](/StreamToImpala/)
-----------------------------

In this Adapter, we will make the events ingested in a CDAP Stream queryable through Impala.


[RDBMS To HBase](/DBTableToHBaseTable/)
----------------

In this Adapter, we will read data from a RDBMS Table and write to HBase Table.


[HBase to RDMBS](/HBaseTableToDBTable/)
----------------

In this Adapter, we will read data from a HBase Table and write to a RDBMS Table.

What You Will Need
==================

- `JDK 7 <http://www.oracle.com/technetwork/java/javase/downloads/index.html>`__
- `CDAP SDK <http://docs.cdap.io/cdap/current/en/developers-manual/getting-started/standalone/index.html>`__

Let's Begin!
============

For these guides, we will use the CDAP CLI to create and manage Adapters. The CLI commands assume that the ``cdap-cli.sh`` script is available on your PATH. 
If this is not the case, please add it::

  $ export PATH=$PATH:<CDAP home>/bin

If you haven't already started a standalone CDAP installation, start it with the command::

  $ cdap.sh start

Now navigate to the Adapter that you want to create and you will find further instructions on how to create that specific Adapter.

Share and Discuss!
==================

Have a question? Discuss at the `CDAP User Mailing List <https://groups.google.com/forum/#!forum/cdap-user>`__.

License
=======

Copyright © 2015 Cask Data, Inc.

Licensed under the Apache License, Version 2.0 (the "License"); you may
not use this file except in compliance with the License. You may obtain
a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

