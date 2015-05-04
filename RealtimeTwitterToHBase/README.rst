Realtime Twitter To HBase Adapter Configuration
===============================================

ETL Realtime Template can be used to create an Adapter that reads from Realtime Source and persists it to Sink. In this example, we will read messages from Twitter in realtime and use a TableSink to write the Tweets to HBase.

The config.json contains a sample Adapter configuration that you can use to accomplish the above task. We choose the etlRealtime application template since we want to create a Realtime Adapter. We choose the source to be Twitter source and provide the OAuth credentials as properties to the source. Since we want to persist the Tweets to HBase, we use a Table Sink. For the Table Sink, we choose the table name to be tweetTable (which will be created if the Table doesn't exist already) and we choose the row key to be id field of the Record emitted by the Twitter Source.

You can create and start the Adapter by using the CDAP CLI (or you can choose the UI to pick and choose the plugins and enter the properties).

cdap> create adapter tweetAdapter /RealtimeTwitterToHBase/config.json
Successfully created adapter 'tweetAdapter'

cdap> start adapter tweetAdapter
Successfully started adapter 'tweetAdapter'

You can verify that the data is being written to the Table by viewing the contents of the Table, tweetTable, using the HBase shell.

You have now successfully created an Adapter that gets Tweet data from Twitter and writes to an HBase Table.

You can stop and delete the Adapter using the CDAP CLI.

cdap> stop adapter tweetAdapter
Successfully stopped adapter 'tweetAdapter'

cdap> delete adapter tweetAdapter
Successfully deleted adapter 'tweetAdapter'

