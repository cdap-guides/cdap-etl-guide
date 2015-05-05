===========================================================
Creating ETL Pipelines using CDAP ETL Application Templates
===========================================================

Using CDAP ETL Application Templates, you can create ETL Pipelines (ETL Adapters) with just a JSON configuration file. 
CDAP ships with a set of built-in Sources, Sinks and Transforms [doc link] which can be used to create Batch ETL and Realtime ETL Adapters right out of the box.

Note: If you want to create your own Source and Sink, you can find more instructions on how to do that here : [doc link]

What You Will Create
====================

Realtime Twitter To HBase <link to dir in github>
-------------------------

In this Adapter, we will read Tweets from Twitter in Realtime and write to HBase Table.


Realtime Kafka To HBase <link to dir in github>
-----------------------

In this Adapter, we will fetch messages from Kafka in Realtime and write to HBase Table.


CDAP Stream To Impala <link to dir in github>
---------------------

In this Adapter, we will make the events ingested in a CDAP Stream queryable through Impala.


What You Will Need
==================

- `JDK 7 <http://www.oracle.com/technetwork/java/javase/downloads/index.html>`__
- `CDAP SDK <http://docs.cdap.io/cdap/current/en/developers-manual/getting-started/standalone/index.html>`__

Let's Begin!
============

For these guides, we will use the CDAP CLI to create and manage Adapters. The CLI commands assume that the cdap-cli.sh script is available on your PATH. 
If this is not the case, please add it:

::

  $ export PATH=$PATH:<CDAP home>/bin

If you haven't already started a standalone CDAP installation, start it with the command:

::

  $ cdap.sh start

Now navigate to the Adapter that you want to create and you will find further instructions on how to create that specific Adapter.

