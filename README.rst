=====================================================
Creating ETL Applications using CDAP System Artifacts
=====================================================

Using the built-in ``cdap-etl-batch`` and ``cdap-etl-realtime`` system artifacts,
you can create ETL Pipelines (ETL Applications) with just a JSON configuration file. 
CDAP ships with a set of built-in Sources, Sinks and Transforms as described `here <http://docs.cask.co/cdap/current/en/cdap-apps/hydrator/index.html>`__
which can be used to create Batch ETL Applications and Real-time ETL Applications right out of the box.

Note: If you want to create your own Source and Sink, you can find more instructions on how to do that `here <http://docs.cask.co/cdap/current/en/cdap-apps/hydrator/custom.html>`__.

What You Will Create
====================

.. |DBTableToHBaseTable| replace:: **Batch Database Table to CDAP HBase Table:**
.. _DBTableToHBaseTable: https://github.com/cdap-guides/cdap-etl-guide/tree/develop/DBTableToHBaseTable

- |DBTableToHBaseTable|_ In this Application, we will export the contents of a Database Table to a CDAP HBase table in Batch.


.. |HBaseTableToDBTable| replace:: **Batch CDAP HBase Table to Database Table:**
.. _HBaseTableToDBTable: https://github.com/cdap-guides/cdap-etl-guide/tree/develop/HBaseTableToDBTable

- |HBaseTableToDBTable|_ This Application exports the contents of a CDAP HBase Table to a Database Table in Batch.


.. |StreamToImpala| replace:: **Batch CDAP Stream To Impala:**
.. _StreamToImpala: https://github.com/cdap-guides/cdap-etl-guide/tree/develop/StreamToImpala

- |StreamToImpala|_ This Application makes the events ingested in a CDAP Stream queryable through Impala.


.. |Real-timeKafkaToHBase| replace:: **Real-time Kafka To HBase:**
.. _Real-timeKafkaToHBase: https://github.com/cdap-guides/cdap-etl-guide/tree/develop/RealtimeKafkaToHBase

- |Real-timeKafkaToHBase|_ With this Application, we will fetch messages from Kafka in real time and write to an HBase Table.


.. |Real-timeTwitterToHBase| replace:: **Real-time Twitter To HBase:**
.. _Real-timeTwitterToHBase: https://github.com/cdap-guides/cdap-etl-guide/tree/develop/RealtimeTwitterToHBase

- |Real-timeTwitterToHBase|_ In this Application, we will read Tweets from Twitter in real time and write to an HBase Table.

.. |RealtimeJMSToStream| replace:: **Real-time JMS To Stream:**
.. _RealtimeJMSToStream: https://github.com/cdap-guides/cdap-etl-guide/tree/develop/RealtimeJMSToStream

- |RealtimeJMSToStream|_ In this Application, we will read messages from a JMS producer in real time and write to a CDAP Stream.

What You Will Need
==================

- `JDK 7 or 8 <http://www.oracle.com/technetwork/java/javase/downloads/index.html>`__
- `CDAP SDK <http://docs.cdap.io/cdap/current/en/developers-manual/getting-started/standalone/index.html>`__

Let's Begin!
============

For these guides, we will use the CDAP CLI to create and manage ETL Applications. The CLI commands assume that the ``cdap-cli.sh`` script is available on your PATH. 
If this is not the case, please add it::

  $ export PATH=$PATH:<CDAP home>/bin

If you haven't already started a standalone CDAP installation, start it with the command::

  $ cdap.sh start

Now navigate to the ETL Application that you want to create and you will find further instructions on how to create that specific ETL Application.

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

