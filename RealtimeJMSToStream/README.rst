====================================================================================
Real-time Java Message Service (JMS) Source to Stream Sink Application Configuration
====================================================================================

The ``cdap-etl-realtime`` system artifact can be used to create an application that reads
from a real-time source and pass it to a sink for further processing.

In this example, we will poll messages from a Java Message Service (JMS) source in real
time and use a Stream sink to write the messages to a CDAP stream.

The file `config.json <config.json>`__ contains a sample application configuration that
you can use to accomplish the above task. Our sample application uses these components:

- The ``cdap-etl-realtime`` system artifact, since we want to perform the pipeline in real time
- JMS source, using the implementation of JMS from Apache ActiveMQ, version 5.11.1
- Stream sink to hold the incoming messages to be processed by a downstream program such as a CDAP flow

You can create and start the application by using the CDAP CLI (or you can use the CDAP
UI for a more visual approach).

**Notes:**

- As ``cdap-etl-realtime`` has been deprecated, this example is deprecated and will be
  updated or removed in a later release.

- You need to fill in the following configurations in a file such as the `config.json
  <config.json>`__ before creating the application.
  
- If you want to import the ``config.json`` into the CDAP UI, you will need to
  modify it to include an ``artifact`` property describing the system artifact being used.
  You can create an initial application as described here using the CLI and then clone it
  in the UI to develop it further.
  
- The real-time JMS source uses a generic JMS interface to communicate with a JMS broker,
  so the source requires that the JMS interfaces be implemented, in order to function as a
  separate plugin.

- You need to use the identical version of JMS for both the client and the target server
  to ensure that the message format is respected when the JMS source start polling messages.


Additional Required Plugins
===========================
The real-time JMS source requires an additional plugin jar that bundles all JMS-specific
implementations and the dependencies into a JAR file. For Apache ActiveMQ, we use the
``activemq-all-5.11.1.jar`` file that contains most of the required dependencies.

For each additional plugin JAR, there should be an accompanying JSON file, with the same
name as the JAR name but with the ``.json`` extension, to describe the plugin. This is so
the application will know of it and load it as part of the plugin class loader.

The JAR and the JSON counterpart files should be put in the ``artifacts`` directory.

There are additional jars that should be copied into the ``artifacts`` directory. These
JARs will be available as part of the combined class loaders for all real-time plugins.
For example, for Apache ActiveMQ, we need to copy the JAR containing the JMS specifications
API. 

If CDAP is running, either restart CDAP (which will reload all artifacts) or use the CDAP
CLI to load the new artifact::

  cdap>  load artifact artifacts/activemq-all-5.11.1.jar
  Successfully added artifact with name 'activemq-all'


Available Properties for JMS Source
===================================
To configure the source, these properties are required:

#. ``referenceName``: This will be used to uniquely identify this source for lineage,
   annotating metadata, etc.

#. ``jms.destination.name``: Name of the JMS destination (either Topic or Queue name) to
   receive messages

#. ``jms.messages.receive``: Maximum number of messages to be retrieved per poll; the
   default value is 50

#. ``jms.factory.initial``: The fully-qualified classname of the factory class that will
   create an initial context

#. ``jms.provider.url``: The URL of the JMS server broker to connect to

#. ``jms.jndi.connectionfactory.name``: The name of the connection factory from the JNDI. 
   The default value is ``ConnectionFactory``.

In addition to the properties expected by the JMS source, you can pass additional
key-value pairs of properties that will be passed through to the underlying JNDI initial
context to configure the implementation of the JMS APIs.

For example, the Apache ActiveMQ requires that the Topic or Queue name be set as the
``topic.<destination name>`` or ``queue.<destination name>`` property key names in order
for the JNDI context to be resolved.

The ``config.json`` file included shows the application configuration file with sample
values for the JMS source properties.


The JMS APIs Implementation Plugin JSON Configuration File
==========================================================
As mentioned above, the JMS source requires an additional plugin that actually performs the 
execution of polling messages from the target server.

The guide provides the ``activemq-all-5.11.1.json`` file as an example based on the Apache
ActiveMQ, version 5.11.1:

#. ``type``: Type of the plugin; as of now, it has to be ``JMSProvider``
#. ``name``: Name of this plugin; as of now, it has to be ``java.naming.factory.initial``
#. ``className``: Fully qualified name of the main class for the intial context factory
   implementation; for ActiveMQ it is ``org.apache.activemq.jndi.ActiveMQInitialContextFactory``
#. ``description``: The description of this plugin


Available Properties for Stream Sink
====================================
To configure the sink, these properties are required:

#. ``name``:  Name of the stream to output to; the stream will be created if it does not already exist
#. ``headers.field``: Name of the header field from the incoming events to be processed by the sink
#. ``body.field``: Nme of the body field from the incoming events to be processed by the sink

In this Adapter, the JMS source will send events to the Stream sink in the ``StructuredRecord`` format, 
with the data stored in a body field called ``message``.

The ``config.json`` file sets ``body.field`` as ``message`` to let the Stream sink know to retrieve 
the data from the ``message`` field of the incoming ``StructuredRecord`` events.


Share and Discuss!
==================
Have a question? Discuss at the `CDAP User Mailing List
<https://groups.google.com/forum/#!forum/cdap-user>`__.


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
