==================================================================
Batch Database Table to CDAP HBase Table Application Configuration
==================================================================

The built-in ``cdap-data-pipeline`` system artifact can be used to create a data pipeline
application that reads from a Batch Source and persists it to a Sink. In this example, we
will read an entire database table in batch and use a TableSink to write the database table's
rows to a CDAP HBase Table.

The file `config.json <config.json>`__ contains a sample application configuration that
you can use to accomplish the above task. Our sample application uses these components:

- The ``cdap-data-pipeline`` system artifact, since we want to perform the pipeline in batch
- Database source, to read data from the Database table 
- Table sink, to write the rows from the Database table to a CDAP HBase table
- A JAR file containing the JDBC driver for your database. With this, you will need 
  a JSON file that describes the JDBC driver as an external plugin. See
  ``mysql-connector-java-5.1.35.json`` and ``postgresql-9.4.json`` as examples.

You can create and start the application by using the CDAP CLI (or you can use the Cask
Hydrator UI for a more visual approach).

**Notes:**

- You need to fill in the following configurations in a file such as the `config.json
  <config.json>`__ before creating the application.
  
- If you want to import the ``config.json`` into the Cask Hydrator UI, you will need to
  modify it to include an ``artifact`` property describing the system artifact being used.
  You can create an initial application as described here using the CLI and then clone it
  in the UI to develop it further.


Configurations for the Database Table Source
============================================

#. ``referenceName``: This will be used to uniquely identify this source for lineage, annotating metadata, etc.

#. ``connectionString``: The JDBC connection string that includes the database name.

#. ``user``: The username used to connect to the specified database. It is 
   required for databases that need authentication, optional for those that do not.

#. ``password``: The password used to connect to the specified database. It is 
   required for databases that need authentication, optional for those that do not.

#. ``importQuery``: The SELECT query to use to import data from the specified table.
   You can specify an arbitrary number of columns to import, or import all columns using \*. The Query should
   contain the '$CONDITIONS' string. For example, 'SELECT * FROM table WHERE $CONDITIONS'.
   The '$CONDITIONS' string will be replaced by 'splitBy' field limits specified by the bounding query.
   The '$CONDITIONS' string is not required if ``numSplits`` is set to one.

#. ``splitBy``: The is the field used to generate splits. Not required if ``numSplits`` is set to one.

#. ``numSplits``: The number of splits to generate (optional).

#. ``boundingQuery``: A bounding query that should return the minimum and maximum of the values of the 'splitBy' field.
   For example, 'SELECT MIN(id),MAX(id) FROM table'. Not required if ``numSplits`` is set to one.

#. ``jdbcPluginName``: The name of the external JDBC plugin. This is the value of the 
   ``name`` field in the external plugin's JSON configuration file. Defaults to 'jdbc'.
   Examples of external plugins are in the files ``mysql-connector-java-5.1.35.json`` and
   ``postgresql-9.4.json``. Refer to the CDAP documentation on external plugins for more
   details.

#. ``jdbcPluginType``: The name of the external JDBC plugin. This is the value of the
   ``type`` field in the external plugin's JSON configuration file. Defaults to 'jdbc'.
   Examples of external plugins are in the files ``mysql-connector-java-5.1.35.json`` and
   ``postgresql-9.4.json``. Refer to the CDAP documentation on external plugins for more
   details.


Configurations for the CDAP HBase Table Sink
============================================

#. ``name``: The name of the HBase table to use as a sink.

#. ``schema``: The JSON representation of the HBase Table's schema.

#. ``schema.row.field``: The field in the ``StructuredRecord`` input to this Sink
   that will be used as the ``rowKey`` in the HBase table.


Creating a Hydrator Application using the CDAP CLI
==================================================
Add the JDBC driver as a plugin artifact available to ``cdap-data-pipeline``::

  cdap> load artifact </path/to/driver.jar> config-file </path/to/config.json>

For example, if you want to use PostgreSQL::

  cdap> load artifact /path/to/postgresql-9.4-1203.jdbc41.jar config-file DBTableToCDAPHBaseTable/postgresql-9.4.json
  Successfully added artifact with name 'postgresql'

Modify ``config.json`` to match your database settings, then create an application
named ``dbIngest`` (replace <version> with your CDAP version)::

  cdap> create app dbIngest cdap-data-pipeline <version> system DBTableToCDAPHBaseTable/config.json
  Successfully created application

  cdap> start workflow dbIngest.ETLWorkflow
  Successfully started workflow 'ETLWorkflow' of application 'dbIngest' with stored runtime arguments '{}'

This will run the workflow once. To schedule the workflow to run periodically::

  cdap> resume schedule dbIngest.etlWorkflow 
  Successfully resumed schedule 'etlWorkflow' in app 'dbIngest'

To verify that the data has been written to the CDAP HBase Table, execute this CDAP CLI
command::

  cdap> execute 'select * from dataset_hbase_postgres_table'

You have now successfully created an application that reads from a Database Table and writes
to a CDAP HBase Table.

To delete the application execute this command using the CDAP CLI::

  cdap> delete app dbIngest
  Successfully deleted application 'dbIngest'


Share and Discuss!
==================
Have a question? Discuss at the `CDAP User Mailing List
<https://groups.google.com/forum/#!forum/cdap-user>`__.

License
=======
Copyright © 2015-2016 Cask Data, Inc.

Licensed under the Apache License, Version 2.0 (the "License"); you may
not use this file except in compliance with the License. You may obtain
a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
