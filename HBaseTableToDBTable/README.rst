==============================================================
CDAP HBase Table To Database Table Batch Adapter Configuration
==============================================================

The ETL Batch Template can be used to create an Adapter that reads from a Batch Source and persists it to a Sink.
In this example, we will read an entire HBase table in batch mode and use a Database Sink to write the HBase 
table's rows to a database.

The config.json contains a sample Adapter configuration that you can use to accomplish the above task. 
Our sample Adapter uses these components:

- ETLBatch Application Template, since we want to perform ETL in batch
- Table source, to read data from the HBase table 
- Database sink, to write the data from the HBase table to a database table
- A jar file containing the JDBC driver for your database. Along with this, you also need a JSON file 
  that describes the JDBC driver as an external plugin. This file should have the same name as the jar file 
  (with only the extension changed to '.json'). See ``mysql-connector-java-5.1.35.json`` and 
  ``postgresql-9.4.json`` as examples.

You can create and start the Adapter by using the CDAP CLI (or you can use the UI for a more visual approach).

Note: You need to fill in the following configurations in a file such as config.json before creating the Adapter.

Configurations for the CDAP HBase Table Source
----------------------------------------------

#. ``name``: This is the name of the HBase table to use as a source.
#. ``schema``: This is the JSON representation of the HBase Table's schema.
#. ``schema.row.field``: Optional field name indicating that the field value should come from the row key 
   instead of a row column. The field name specified must be present in the schema, and must not be nullable.

Configurations for the Database Table Sink
------------------------------------------

#. ``driverClass``: This is the JDBC driver class used to connect to your database.
#. ``connectionString``: This is the JDBC connection string that includes the database name.
#. ``tableName``: This is the table to which you wish to export.
#. ``user``: This is the username used to connect to the specified database. It is required for databases 
   that need authentication, optional for those that do not.
#. ``password``: This is the password used to connect to the specified database. If the database requires 
   authentication, both username and password must be provided.
#. ``columns``: This is a comma-separated list of columns in the database table to which data from the 
   HBase table will be exported.
#. ``jdbcPluginName``: The name of the external JDBC plugin. This is the value of the ``name`` field in 
   the external plugin's JSON configuration file. Defaults to 'jdbc'. Please find examples of external plugins
   in the files ``mysql-connector-java-5.1.35.json`` and ``postgresql-9.4.json``. Also refer to the CDAP 
   documentation on external plugins for more details.
#. ``jdbcPluginType``: The name of the external JDBC plugin. This is the value of the ``type`` field in 
   the external plugin's JSON configuration file. Defaults to 'jdbc'. Please find examples of external plugins 
   in the files ``mysql-connector-java-5.1.35.json`` and ``postgresql-9.4.json``. Also refer to the CDAP 
   documentation on external plugins for more details.

Creating an adapter using CDAP CLI
----------------------------------

::

  cdap> create adapter dbExport HBaseTableToDBTable/config.json
  Successfully created adapter 'dbExport'

  cdap> start adapter dbExport
  Successfully started adapter 'dbExport'

To verify that the data has been written to the Database Table execute the following SQL command on your Database::

  select * from dest_db_table

You have now successfully created an Adapter that reads from an HBase Table and writes to a Database Table.

To stop and delete the Adapter execute the following commands using the CDAP CLI::

  cdap> stop adapter dbExport
  Successfully stopped adapter 'dbExport'

  cdap> delete adapter dbExport
  Successfully deleted adapter 'dbExport'


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
