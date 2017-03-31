====================================================
Real-time Twitter to HBase Application Configuration
====================================================

If you want to fetch Tweets from Twitter in real time and persist them in an HBase Table,
you can using a CDAP pipeline. The ``cdap-data-streams`` system artifact can be used to create an
Application that reads from a real-time source and persists it to a real-time sink. In
this example, we will read messages from Twitter and use a TableSink to write the Tweets
to HBase.

Configuration
=============
The file `config.json <config.json>`__ contains a sample Application configuration that
you can use to accomplish the above task.

You can create and start the Application by using the CDAP CLI (or you can use the CDAP
UI for a more visual approach).

**Notes:**

- If you want to import the ``config.json`` into the CDAP UI, you will need to
  adjust the version of the ``artifact`` property to described the system artifact being
  used. You can create an initial application as described here using the CLI and then
  clone it in the UI to develop it further.
  
- You need to complete the source and sink configurations as described below before
  creating your application.

Source
------
We choose the Twitter source and provide the OAuth credentials as properties to the source.

For selecting the source, we use the "source" property. The Twitter source expects four properties 
(OAuth credentials for Twitter):

- ``AccessToken``
- ``AccessTokenSecret``
- ``ConsumerKey``
- ``ConsumerSecret``

Note: You need to fill in the OAuth credentials in ``config.json`` before creating the
Application. You can visit `Twitter <https://dev.twitter.com>`__ and
`dev.Twitter <https://dev.twitter.com/oauth/overview/application-owner-access-tokens>`__
for information on how to obtain OAuth credentials for the Twitter source.

Sink
----
Since we want to persist the Tweets to HBase, we use a Table sink. For the Table sink, we
set the table name to be ``tweetTable`` (which will be created if the Table doesn't
already exist) and we set the row key to be the ``id`` field of the record emitted by the
Twitter source:

- ``name``: Name of the Table dataset
- ``schema.row.field``: The field which is used as the row key in the Table

Creating and Running
====================
Create a pipeline application named ``tweetApp`` (replace <version> with your CDAP version) using the CDAP CLI::

  cdap> create app tweetApp cdap-data-streams <version> system RealtimeTwitterToHBase/config.json
  Successfully created application

  cdap> start spark tweetApp.DataStreamsSparkStreaming
  Successfully started Spark program 'DataStreamsSparkStreaming' of application 'tweetApp' with stored runtime arguments '{}'


You can verify that the data is being written to the ``tweetTable`` dataset by executing a query against it::

  cdap> execute 'select message from dataset_tweetTable limit 5'
  +================================================================================================================+
  | message: STRING                                                                                                |
  +================================================================================================================+
  | Keating was the last PM to increase benefit, and that was a $4/week rise. @ACOSS #auwu2016                     |
  |----------------------------------------------------------------------------------------------------------------|
  | RT @ReverieHippie: Everything serves a purpose. Continue to learn and grow on your journey.                    |
  |----------------------------------------------------------------------------------------------------------------|
  | RT @MusaInspirador: NO Debes Besar Tu hijo En La Boca, Mira Por Qu\xC3\xA9?.\x0A\x0AV\xC3\x8DDEO               |
  |----------------------------------------------------------------------------------------------------------------|
  | @Vicente77990064 @tuyacandela  keeeeee? Quien te llamo, mamerto                                                |
  |----------------------------------------------------------------------------------------------------------------|
  | Espero que kevin ma\xC3\xB1ana me valla a buscar con su cel cargado porque pienso sacarme muchas fotos con el  |
  +================================================================================================================+
  Fetched 5 rows

You have now successfully created an Application that retrieves Tweet data from Twitter and writes to an HBase Table.


Stopping and Deleting
---------------------
You can stop and delete the Application using the CDAP CLI::

  cdap> stop spark tweetApp.DataStreamsSparkStreaming
  Successfully stopped Spark program 'DataStreamsSparkStreaming' of application 'tweetApp'

  cdap> delete app tweetApp
  Successfully deleted application 'tweetApp'


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

