.. _services_harvesting:

Harvesting services
===================

Introduction
------------

This chapter provides a detailed explanation of the GeoNetwork’s harvesting
services. These services allow a complete control over the harvesting behaviour.
They are used by the web interface and can be used by any other client.

xml.harvesting.get
------------------

Retrieves information about one or all configured harvesting nodes.

Request
```````

Called with no parameters returns all nodes. Example::

    <request/>

Otherwise, an id parameter can be specified::

    <request>
        <id>123</id>
    </request>

Response
````````

When called with no parameters the service provide its output inside a
nodes container. You get as many node elements as are configured. 

**Example of an xml.harvesting.get response for a GeoNetwork node**::

    <nodes>
        <node id="125" type="geonetwork">
            <site>
                <name>test 1</name>
                <uuid>0619cc50-708b-11da-8202-000d9335aaae</uuid>
                <account>
                    <use>false</use>
                    <username />
                    <password />
                </account>
                <host>http://www.fao.org/geonetwork</host>
                <createRemoteCategory>true</createRemoteCategory>
                <mefFormatFull>true</mefFormatFull>
                <xslfilter/>
            </site>
            <content>
                <validate>true</validate>
                <importxslt>none</importxslt>
            </content>
            <options>
                <every>0 0 0/3 ? * *</every>
                <oneRunOnly>false</oneRunOnly>
                <status>inactive</status>
            </options>
            <searches>
                <search>
                    <freeText />
                    <title />
                    <abstract />
                    <keywords />
                    <digital>false</digital>
                    <hardcopy>false</hardcopy>
                    <source>
                        <UUID>0619cc50-708b-11da-8202-000d9335906e</uuid>
                        <name>Food and Agriculture organisation</name>
                    </source>
                </search>
            </searches>
            <groupsCopyPolicy>
                <group name="all" policy="copy"/>
                <group name="mygroup" policy="createAndCopy"/>
            </groupsCopyPolicy>
            <categories>
                <category id="4"/>
            </categories>
            <info>
                <lastRun />
                <running>false</running>
            </info>
        </node>
    </nodes>

If you specify an id, you get a response like the one below.

**Example of an xml.harvesting.get response for a WebDAV node**::

    <node id="165" type="webdav">
        <site>
            <name>test 1</name>
            <UUID>0619cc50-708b-11da-8202-000d9335aaae</uuid>
            <url>http://www.mynode.org/metadata</url>
            <icon>default.gif</icon>
            <account>
                <use>true</use>
                <username>admin</username>
                <password>admin</password>
            </account>
        </site>
        <options>
            <every>0 0 0/3 ? * *</every>
            <oneRunOnly>false</oneRunOnly>
            <recurse>false</recurse>
            <validate>true</validate>
            <status>inactive</status>
        </options>
        <privileges>
            <group id="0">
                <operation name="view" />
            </group>
            <group id="14">
                <operation name="download" />
            </group>
        </privileges>
        <categories>
            <category id="2"/>
        </categories>
        <info>
            <lastRun />
            <running>false</running>
        </info>
    </node>

The node structure for all harvesters has some common XML elements, plus 
additional elements that are specific to each harvesting type.

The common XML elements are described at :ref:`harvesting_nodes`.

Errors
``````

- ObjectNotFoundEx If the id parameter is provided but the node
  cannot be found.

xml.harvesting.add
------------------

Create a new harvesting node. The node can be of any type supported by
GeoNetwork (GeoNetwork node, web folder etc...). When a new node is created, its
status is set to inactive. A call to the xml.harvesting.start service is
required to start harvesting.

Request
```````

The service requires an XML tree with all information about the harvesting node to be added. The common XML elements that must be in the tree are described at :ref:`harvesting_nodes`. Settings and example requests for each type of harvester in GeoNetwork are as follows:

- :ref:`geonetwork_harvesting`
- :ref:`webdav_harvesting`
- :ref:`csw_harvesting`
- :ref:`z3950_harvesting`
- :ref:`oaipmh_harvesting`
- :ref:`thredds_harvesting`
- :ref:`wfsfeatures_harvesting`
- :ref:`filesystem_harvesting`
- :ref:`arcsde_harvesting`

Summary of features of the supported harvesting types
.....................................................

===============     ==============      ================    ============
Harvesting type     Authentication      Privileges          Categories
===============     ==============      ================    ============
GeoNetwork          native              through policies    yes
WebDAV              HTTP digest         yes                 yes
CSW                 HTTP Basic          yes                 yes
===============     ==============      ================    ============

xml.harvesting.update
---------------------

This service is responsible for changing the node’s parameters. A typical
request has a node root element and must include the id attribute::

    <node id="24">
        ...
    </node>

The body of the node element depends on the node’s type. The update policy is
this:

- If an element is specified, the associated parameter is updated.

- If an element is not specified, the associated parameter will not be
  changed.

So, you need to specify only the elements you want to change. However, there
are some exceptions:

#.  **privileges**: If this element is omitted, privileges will not be changed. If
    specified, new privileges will replace the old ones.

#.  **categories**: Like the previous one.

#.  **searches**: Some harvesting types support multiple searches on the
    same remote note. When supported, the updated behaviour should be like the
    previous ones.

Note that you cannot change the type of an node once it has been created.

Request
```````

The request is the same as that used to add an entry. Only the id
attribute is mandatory.

Response
````````

The response is the same as the xml.harvesting.get called on the updated
entry.

xml.harvesting.remove /start /stop /run
---------------------------------------

These services are put together because they share a common request interface.
Their purpose is obviously to remove, start, stop or run a harvesting node. In
detail:

#.  **remove**: Remove a node. Completely deletes the harvesting instance.

#.  **start**: When created, a node is in the inactive state. This operation makes it
    active, that is the countdown is started and the harvesting will be performed at
    the timeout.

#.  **stop**: Makes a node inactive. Inactive nodes are never harvested.

#.  **run**: Just start the harvester now. Used to test the harvesting.

Request
```````

A set of ids to operate on. Example::

    <request>
        <id>123</id>
        <id>456</id>
        <id>789</id>
    </request>

If the request is empty, nothing is done.

Response
````````

The same as the request but every id has a status attribute indicating the
success or failure of the operation. For example, the response to the
previous request could be::

    <request>
        <id status="ok">123</id>
        <id status="not-found">456</id>
        <id status="inactive">789</id>
    </request>

:ref:`table_service_status2` summarises, for each service, the
possible status values.

.. _table_service_status2:

Summary of status values
........................

.. |ok| image:: button_ok.png

================    ======  =====   ====    ====
Status value        remove  start   stop    run
================    ======  =====   ====    ====
ok                  |ok|    |ok|    |ok|    |ok|
not-found           |ok|    |ok|    |ok|    |ok|
inactive                                    |ok|
already-inactive                    |ok|    
already-active              |ok|            
already-running                             |ok|
================    ======  =====   ====    ====

