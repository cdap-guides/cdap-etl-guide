==================================================================
Batch Database Table To CDAP HBase Table Application Configuration
==================================================================

The built-in ``cdap-etl-batch`` system artifact can be used to create an ETL Application
that reads from a Batch Source and persists it to a Sink.
In this example, we will read an entire DB table in batch and use a
TableSink to write the database table's rows to HBase.

The ``config.json`` contains a sample Application configuration that you can use to accomplish the
above task. Our sample Application uses these components:

- The ``cdap-etl-batch`` system artifact, since we want to perform ETL in batch
- Database source, to read data from the Database table 
- Table sink, to write the rows from the Database table to an HBase table
- A jar file containing the JDBC driver for your database. Along with this, you also need 
  a JSON file that describes the JDBC driver as an external plugin. See
  ``mysql-connector-java-5.1.35.json`` and ``postgresql-9.4.json`` as examples.

You can create and start the Application by using the CDAP CLI (or you can use the UI for a
more visual approach).

Note: You need to fill in the following configurations in a file such as ``config.json``
before creating the Application.

Configurations for the Database Table Source
--------------------------------------------

#. ``connectionString``: This is the JDBC connection string that includes the database name.

#. ``tableName``: This is the table from which you wish to import.

#. ``user``: This is the username used to connect to the specified database. It is 
   required for databases that need authentication, optional for those that do not.

#. ``password``: This is the password used to connect to the specified database. If the 
   database requires authentication, both username and password must be provided.

#. ``importQuery``: This is the SELECT query used to import data from the specified table. 
   You can specify an arbitrary number of columns to import, or import all columns using
   \*. You can also specify a number of WHERE clauses or ORDER BY clauses. However, LIMIT
   and OFFSET clauses should not be used in this query.

#. ``countQuery``: This is the SELECT query used to get the count of records to import 
   from the specified table. Examples: SELECT COUNT(*) from <my_table> where <my_column>
   1, SELECT COUNT(my_column) from my_table). NOTE: Please include the same WHERE clauses
   in this query as the ones used in the import query to reflect an accurate number of
   records to import.

#. ``jdbcPluginName``: The name of the external JDBC plugin. This is the value of the 
   ``name`` field in the external plugin's JSON configuration file. Defaults to 'jdbc'.
   Please find examples of external plugins in the files
   ``mysql-connector-java-5.1.35.json`` and ``postgresql-9.4.json``. Also refer to the
   CDAP documentation on external plugins for more details.

#. ``jdbcPluginType``: The name of the external JDBC plugin. This is the value of the
   ``type`` field in the external plugin's JSON configuration file. Defaults to 'jdbc'.
   Please find examples of external plugins in the files
   ``mysql-connector-java-5.1.35.json`` and ``postgresql-9.4.json``. Also refer to the
   CDAP documentation on external plugins for more details.

Configurations for the CDAP HBase Table Sink
--------------------------------------------

#. ``name``: This is the name of the HBase table to use as a sink.

#. ``schema``: This is the JSON representation of the HBase Table's schema.

#. ``schema.row.field``: This is the field in the ``StructuredRecord`` input to this Sink
   that will be used as the ``rowKey`` in the HBase table.

Creating an ETL Application using CDAP CLI
------------------------------------------
Add the JDBC driver as a plugin artifact available to ``cdap-etl-batch``::

  cdap> load artifact </path/to/driver.jar> config-file </path/to/config.json>

For example, if you want to use postgresql::

  cdap> load artifact /path/to/postgresql-9.4-1203.jdbc41.jar config-file DBTableToHBaseTable/postgresql-9.4.json
  Successfully added artifact with name 'postgresql'

Create an ETL Application named ``dbIngest`` (replace <version> with your CDAP version)::

  cdap> create app dbIngest cdap-etl-batch <version> system DBTableToHBaseTable/config.json
  Successfully created application

  cdap> start workflow dbIngest.ETLWorkflow
  Successfully started workflow 'ETLWorkflow' of application 'dbIngest' with stored runtime arguments '{}'

This will run the workflow once. To schedule the workflow to run periodically::

  cdap> resume schedule dbIngest.etlWorkflow 
  Successfully resumed schedule 'etlWorkflow' in app 'dbIngest'

To verify that the data has been written to the HBase Table execute the following CDAP CLI
command::

  cdap> execute 'select * from dataset_hbase_postgres_table'

You have now successfully created an ETL Application that reads from a Database Table and writes
to a CDAP HBase Table.

To delete the Application execute the following command using the CDAP CLI::

  cdap> delete app dbIngest
  Successfully deleted application 'dbIngest'


Share and Discuss!
==================

Have a question? Discuss at the `CDAP User Mailing List
<https://groups.google.com/forum/#!forum/cdap-user>`__.

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
