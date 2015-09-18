Realtime Twitter To HBase Application Configuration
===================================================

Introduction
------------

Let's say we want to fetch Tweets from Twitter in Realtime and persist them in an HBase Table. 
The cdap-etl-realtime system artifact can be used to create an Application that reads from a Realtime Source and persists it to a Realtime Sink. 
In this example, we will read messages from Twitter and use a TableSink to write the Tweets to HBase.


Configuration
-------------

The ``config.json`` contains a sample Application configuration that you can use to accomplish the above task. 

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

Since we don't use any transforms in this Application, we leave that as an empty array.


Creating and Running
--------------------

You can create and start the Application by using the CDAP CLI (or the UI, for a more visual approach).
Note: You need to fill in the OAuth credentials in ``config.json`` before creating the Adapter. You can 
visit `Twitter <https://dev.twitter.com>`__ for more information on how to obtain OAuth credentials for the Twitter Source.

::

  cdap> create app tweetApp cdap-etl-realtime <version> system RealtimeTwitterToHBase/config.json
  Successfully created application

  cdap> start worker tweetApp.ETLWorker
  Successfully started worker 'ETLWorker' of application 'tweetAdapter' with stored runtime arguments '{}'


You can verify that the data is being written to the Table by viewing the contents of the Table, 
``tweetTable``, using the HBase shell.

You have now successfully created an Application that retrieves Tweet data from Twitter and writes to an HBase Table.


Stopping and Deleting
---------------------

You can stop and delete the Application using the CDAP CLI.

::

  cdap> stop worker tweetApp.ETLWorker
  Successfully stopped worker 'ETLWorker' of application 'tweetApp'

  cdap> delete adapter tweetApp
  Successfully deleted application 'tweetAdapter'

