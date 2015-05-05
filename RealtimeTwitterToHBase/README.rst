Realtime Twitter To HBase Adapter Configuration
===============================================

Introduction
------------

Let's say we want to fetch Tweets from Twitter in Realtime and persist them in an HBase Table. 
ETL Realtime Template can be used to create an Adapter that reads from a Realtime Source and persists it to a Realtime Sink. 
In this example, we will read messages from Twitter and use a TableSink to write the Tweets to HBase.


Configuration
-------------

The ``config.json`` contains a sample Adapter configuration that you can use to accomplish the above task. 
We choose the ETL Realtime Application Template since we want to create a Realtime Adapter. 
We set the Application Template name using the "template" property.

Source
------

We choose the Twitter Source and provide the OAuth credentials as properties to the Source. Since we 
want to persist the Tweets to HBase, we use a Table Sink. For the Table Sink, we set the table name 
to be ``tweetTable`` (which will be created if the Table doesn't exist already) and we set the row key to 
be the ``id`` field of the Record emitted by the Twitter Source.

For selecting the Source, we use the "source" property. The Twitter Source expects four properties 
(OAuth credentials for Twitter):

- ``AccessToken``
- ``AccessTokenSecret``
- ``ConsumerKey``
- ``ConsumerSecret``

Sink
----

Similarly, for selecting the Sink, we use the "sink" property. The Table Sink expects two properties:

- ``name``: Name of the Table Dataset.
- ``schema.row.field``: The field which is used as the row key in the Table.

Since we don't use any transforms in this Adapter, we leave that as an empty array.


Creating and Running
--------------------

You can create and start the Adapter by using the CDAP CLI (or the UI, for a more visual approach).
Note: You need to fill in the OAuth credentials in ``config.json`` before creating the Adapter. You can 
visit `Twitter <https://dev.twitter.com>`__ for more information on how to obtain OAuth credentials for the Twitter Source.

::

  cdap> create adapter tweetAdapter RealtimeTwitterToHBase/config.json
  Successfully created adapter 'tweetAdapter'

  cdap> start adapter tweetAdapter
  Successfully started adapter 'tweetAdapter'


You can verify that the data is being written to the Table by viewing the contents of the Table, 
``tweetTable``, using the HBase shell.

You have now successfully created an Adapter that retrieves Tweet data from Twitter and writes to an HBase Table.


Stopping and Deleting
---------------------

You can stop and delete the Adapter using the CDAP CLI.

::

  cdap> stop adapter tweetAdapter
  Successfully stopped adapter 'tweetAdapter'

  cdap> delete adapter tweetAdapter
  Successfully deleted adapter 'tweetAdapter'

