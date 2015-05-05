===============================================================================
Realtime Java Message Service (JMS) Source To Stream Sink Adapter Configuration
===============================================================================

The ETL Realtime Template can be used to create an Adapter that reads from a Realtime Source and pass it to a Sink to be processed further.

In this example, we will poll messages from Java Message Service (JMS) source in realtime. and use a Stream Sink to write the messages to CDAP Stream.

The config.json contains a sample Adapter configuration that you can use to accomplish the above task. 

Our sample Adapter uses these components:

- ETLRealtime Application Template, since we want to perform ETL in realtime
- JMS source, using the implementation of JMS from Apache ActiveMQ, version 5.11.1
- Stream sink to hold the incoming messages to be processed by a downstream program such as a CDAP Flow

You can create and start the Adapter by using the CDAP CLI (or you can use the UI for a more visual approach).

The realtime JMS source uses generic JMS interface to communicate with JMS broker, so the source requires implementation of the JMS interfaces
to be able to work properly as separate adapter plugin.

One important thing to remember you need to use the identical version of JMS for both the client and the target server to ensure 
that the message format is respected when the JMS source start polling messages.

The realtime JMS source requires an additional plugin jar that bundle all JMS specific implementations into a jar. 
For Apache ActiveMQ, we will be using the ``activemq-all-5.11.1.jar`` file that contains most of the required dependencies.

For each additionl plugin jars, there should be accompanying JSON file with the same name as the jar name, but with .json extension, 
to describe about the plugin so the adapter knows about it and load it as part of the plugon class loader. The jar and the JSON counterpart files
should be put in the templates/plugins/ETLRealtime directory.

There are additional jars that should be copied into templates/plugins/ETLRealtime/lib directory. These jars will be available as part of combined 
class loaders for all realtime plugins so need to copy jar containing JMS specifications API, for example 

Available Properties for JMS Source
===================================

To configure the source, there are some properties that are needed to make it work properly:

#. ``jms.destination.name``: Name of the JMS destination (either Topic or Queue name) to get messages

#. ``jms.messages.receive``: This is the Max number messages should be retrieved per poll. The default value is 50

#. ``jms.factory.initial``: The fully qualified class name of the factory class that will create an initial context

#. ``jms.provider.url``: This is the URL of the JMS server broker to connect to

#. ``ConnectionFactory``: Change the format of the name for the connectionfactory in JMS source plugin. The default value is ConnectionFactory. 

In addition to the properties expected by the JMS source, you could always pass adiitional key-value pairs of properties that will be 
passed to underlyong JNDI initial context to configure the implementation of the JMS APIs.
For example, the Apache ActiveMQ requires the Topic or Queue name to be set as topic.<destination name> or queue.<destination name> property key names
in order for the JNDI context to be resolved.

The ``config.json`` file included shows the Adapter configuration file with sample values for the JMS source properties.

The JMS APIs Implementation Plugin JSON Configuration File
===========================================================

As mentioned before, the JMS source requires additional plugin to actually does the execution of polling the messages from the target server.

The guide provides activemq-all-5.11.1.json file as an example based on Apache ActiveMQ 5.11.1 version:

#. ``type``: This is the type of the plugin. As of now it has to be JMSProvider.
#. ``name``: The name of this plugin. As of now it has to be java.naming.factory.initial.
#. `className`: 
#. `description`: The description of this plugin.

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
