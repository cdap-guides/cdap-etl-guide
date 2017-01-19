==========================================================
Creating Hydrator Applications using CDAP System Artifacts
==========================================================

Using the built-in ``cdap-data-pipeline`` and ``cdap-data-streams`` system artifacts, you can
create Cask Hydrator Pipelines (Hydrator Applications) with just a JSON configuration file. CDAP ships
with a set of built-in Sources, Sinks, Transforms, and other plugins (described `here
<http://docs.cdap.io/cdap/current/en/hydrator-manual/plugins/index.html>`__) which can be used
to create batch and real-time data pipeline applications right out of the box.

Note: If you want to create your own Source, Sink, or other plugin, you can find more
instructions on how to do that `here
<http://docs.cdap.io/cdap/current/en/hydrator-manual/developing-plugins/index.html>`__.

**Note:** Both the ``cdap-etl-batch`` and ``cdap-etl-realtime`` system artifacts have been
deprecated as of CDAP 3.5.0 and replaced with the artifacts ``cdap-data-pipeline`` and
``cdap-data-streams`` respectively.


What You Will Create
====================

.. |CDAPTableToDBTable| replace:: **Batch CDAP HBase Table to Database Table:**
.. _CDAPTableToDBTable: CDAPTableToDBTable

- |CDAPTableToDBTable|_ This application exports the contents of a CDAP HBase Table to a Database Table in Batch.


.. |DBTableToCDAPTable| replace:: **Batch Database Table to CDAP HBase Table:**
.. _DBTableToCDAPTable: DBTableToCDAPTable

- |DBTableToCDAPTable|_ This application exports the contents of a Database Table to a CDAP HBase table in Batch.


.. |StreamToImpala| replace:: **Batch CDAP Stream to Impala:**
.. _StreamToImpala: StreamToImpala

- |StreamToImpala|_ This application makes the events ingested in a CDAP Stream queryable through Impala.


.. |RealtimeJMSToStream| replace:: **Real-time JMS to Stream:**
.. _RealtimeJMSToStream: RealtimeJMSToStream

- |RealtimeJMSToStream|_ This application reads messages from a JMS producer in real time and writes to a CDAP Stream.


.. |RealtimeKafkaToTPFSAvro| replace:: **Real-time Kafka to TPFS Avro:**
.. _RealtimeKafkaToTPFSAvro: RealtimeKafkaToTPFSAvro

- |RealtimeKafkaToTPFSAvro|_ This application fetches messages from Kafka in real time and writes to Time-PartitionedFileSets in Avro format.


.. |RealtimeTwitterToHBase| replace:: **Real-time Twitter to HBase:**
.. _RealtimeTwitterToHBase: RealtimeTwitterToHBase

- |RealtimeTwitterToHBase|_ This application reads Tweets from Twitter in real time and write to an HBase Table.


What You Will Need
==================

- `JDK 7 or 8 <http://www.oracle.com/technetwork/java/javase/downloads/index.html>`__
- `CDAP SDK <http://docs.cdap.io/cdap/current/en/developers-manual/getting-started/standalone/index.html>`__


Let's Begin!
============
For these guides, we will use the CDAP CLI to create and manage Hydrator Applications. The CLI
commands assume that the ``cdap`` script is available on your PATH. If this is not
the case, please add it::

  $ export PATH=$PATH:<CDAP home>/bin
  
or, from within the <CDAP home> directory::

  $ export PATH=${PATH}:`pwd`/bin

If you haven't already started a standalone CDAP installation, start it with the command::

  $ cdap sdk start

Now navigate to the Hydrator Application that you want to create and you will find further
instructions on how to create that specific application:

- |CDAPTableToDBTable|_ This application exports the contents of a CDAP HBase Table to a Database Table in Batch.

- |DBTableToCDAPTable|_ This application exports the contents of a Database Table to a CDAP HBase table in Batch.

- |StreamToImpala|_ This application makes the events ingested in a CDAP Stream queryable through Impala.

- |RealtimeJMSToStream|_ This application reads messages from a JMS producer in real time and writes to a CDAP Stream.

- |RealtimeKafkaToTPFSAvro|_ This application fetches messages from Kafka in real time and writes to Time-PartitionedFileSets in Avro format.

- |RealtimeTwitterToHBase|_ This application reads Tweets from Twitter in real time and write to an HBase Table.


Share and Discuss!
==================
Have a question? Discuss at the `CDAP User Mailing List <https://groups.google.com/forum/#!forum/cdap-user>`__.

License
=======
Copyright © 2015-2017 Cask Data, Inc.

Licensed under the Apache License, Version 2.0 (the "License"); you may
not use this file except in compliance with the License. You may obtain
a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

