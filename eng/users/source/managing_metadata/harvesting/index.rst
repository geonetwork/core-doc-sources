.. _harvesting:

Harvesting
==========

Introduction
------------

Since the beginning of the project, there has been the need to share metadata
among several GeoNetwork nodes. Usually, each node takes care of a region of
interest so it is important to be able to perform a search over all these nodes at
the same time. This is called distributed search and exploits the Internet connectivity. In our cases, this distributed search can be heavy to perform if there are many maps with associated thumbnails. Furthermore, GeoNetwork is usually employed in areas (like Africa, Asia) where the connectivity can be limited, making the use of distributed search not feasible.

Harvesting is the process of collecting remote metadata and storing them locally for a faster access. This is a periodic process to do, for example, once a week. Harvesting is not a simple import: local and remote metadata are kept aligned. Using some *magic*, one GeoNetwork node is capable of discovering metadata that have been added, removed or updated in the remote node.

GeoNetwork is able to harvest from the following sources (for more details see below):

#. Another GeoNetwork node (version 2.1 or above). See :ref:`harvesting_gn`
#. An old GeoNetwork 2.0 node (deprecated). See :ref:`gn2_harvester`
#. A WebDAV server. See :ref:`webdav_harvester`
#. A CSW 2.0.1 or 2.0.2 catalogue server. See :ref:`csw_harvester`
#. An OAI-PMH server. See :ref:`oaipmh_harvester`
#. An OGC service using its GetCapabilities document. These include WMS, WFS, WPS and WCS services. See :ref:`ogcwxs_harvester`
#. An ArcSDE server. See :ref:`sde_harvester`
#. A THREDDS catalog. See :ref:`thredds_harvester`
#. An OGC WFS using a GetFeature query. See :ref:`getfeature_harvester`
#. One or more Z3950 servers. See :ref:`z3950_harvester`

Mechanism overview
------------------

The harvesting mechanism is based on the concept of a *universally unique identifier (UUID)*.
This is a special id because it is not only
unique locally to the node that generated it but it is unique across all the world.
It is a combination of the network interface’s MAC address, the current date/time
and a random number. Every time you create a new metadata in GeoNetwork, a new UUID
is generated and assigned to it.

Another important concept behind the harvesting is the *last change date*.
Every time you change a metadata, its last change date is
updated. Just storing this parameter and comparing it with a new one allows any
system to find out if the metadata has been modified since last update.

These two concepts allow GeoNetwork to fetch a remote metadata, check if it has
been updated and remove it locally if it has been removed remotely. Furthermore,
thanks to UUIDs, a hierarchy of harvesting nodes can be built where B harvests from
C and A harvests from B. Even loops can be created because harvested metadata cannot
be modified.

Harvesting life cycle
---------------------

When a harvesting node is set, there is no harvested metadata. During the first
run, all remote matching metadata are retrieved and stored locally. After the first
run, only changed metadata are retrieved. Harvested metadata are not editable for
the following reasons:

#. The harvesting is periodic so any local change to harvested metadata will be lost during the next run.
#. The change date is used to keep track of changes so if it gets changed outside the originator site, the harvesting mechanism is compromised.

Beside the metadata itself, this implies that users cannot change all other metadata properties (like categories, privileges etc...).

The harvesting process goes on until one of the following situations arises:

#. An administrator stops (deactivates) the node.
#. An exception arises. In this case the node is automatically stopped.

When a harvesting node is removed, all harvest metadata are removed too.

Multiple harvesting and hierarchies
-----------------------------------

Catalogues that provide UUIDs for metadata (for example GeoNetwork and a CSW
server) can be harvested several times without having to take care about metadata
overlap. This allows the possibility to perform a thematic search and a metadata
belonging to multiple searches is harvested only once and not duplicated.

This mechanism allows the GeoNetwork harvesting type to be combined with other
GeoNetwork nodes to perform hierarchical harvesting. This way a metadata can be
harvested from several nodes. For example, consider this scenario:

#. Node (A) has created metadata (a)
#. Node (B) harvests (a) from (A)
#. Node (C) harvests (a) from (B)
#. Node (D) harvests from both (A), (B) and (C)

In this scenario, Node (D) will get the same metadata (a) from all 3 nodes (A),
(B), (C). The metadata will flow to (D) following 3 different paths but thanks to
its UUID only one copy will be stored. When (a) will be changed in (A), a new
version will flow to (D) but, thanks to the change date, the copy in (D) will be
updated with the most recent version.

.. _harvesting_fragments:

Harvesting Fragments of Metadata to support re-use
``````````````````````````````````````````````````

All the harvesters except for the THREDDS and OGC WFS GetFeature harvester create a complete metadata record that is inserted into or replaces an existing record in the catalog. However, it's often the case that:

- the metadata harvested from an external source is really only one or more fragments of the metadata required to describe a resource such as a dataset 
- you might want to combine harvested fragments of metadata with manually entered or static metadata in a single record
- a fragment of metadata harvested from an external source may be required in more than one metadata record

For example, you may only be interested in harvesting the geographic extent and/or contact information from an external source and manually entering or maintaining the remainder of the content in the metadata record. You may also be interested in re-using the contact information for a person or organisation in more than one metadata record.

To support this capability, both the WFS GetFeature Harvester and the THREDDS harvester, allow fragments of metadata to be harvested and linked or copied into a template record to create metadata records. Fragments that are saved into the GeoNetwork database are called subtemplates and can be used in more than one metadata record. These concepts are shown in the diagram below.

.. figure:: web-harvesting-fragments.png

		*Harvesting Metadata Fragments*

As shown above, an example of a metadata fragment is the gmd:contactInfo element of an iso19139 document.  This element contains contact details for an individual or an organisation.  If a fragment is stored in the geonetwork database as a subtemplate for a given person or organisation, then this fragment can be referenced in metadata records where this organisation or individual is specified using an XML linking mechanism called XLink. An example of an XLink is shown in the following diagram.

.. figure:: web-harvesting-xlinks.png


HTTPS support
`````````````

Harvesters support https protocol. If the server to harvest has a trusted certificate available in JVM keystore, configuring the https url in the harvester should be fine.

For self-signed/untrusted certificates, the harvester issues an exception like this::

    javax.net.ssl.SSLHandshakeException: 
       sun.security.validator.ValidatorException: PKIX path building failed: 
       sun.security.provider.certpath.SunCertPathBuilderException: 
       unable to find valid certification path to requested target
     
    Caused by: sun.security.validator.ValidatorException: 
       PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: 
       unable to find valid certification path to requested target
     
    Caused by: sun.security.provider.certpath.SunCertPathBuilderException: 
       unable to find valid certification path to requested target

This indicates that the server certificate requires to be imported in the JVM keystore with `keytool <http://docs.oracle.com/javase/6/docs/technotes/tools/solaris/keytool.html>`_ to be trusted.

An alternative way to import the certificate is to use a script like::

    ## JAVA SSL Certificate import script
    ## Based on original MacOs script by LouiSe@louise.hu : http://louise.hu
    ##
    ## Usage: ./ssl_key_import.sh <sitename> <port>
    ##
    ## Example: ./ssl_key_import.sh mail.google.com 443 (to read certificate from https://mail.google.com)
     
    ## Compile and start 
    javac InstallCert.java
    java InstallCert $1:$2
     
    ## Copy new cert into local JAVA keystore
    echo "Please, enter admnistrator password:"
    sudo cp jssecacerts $JAVA_HOME/jre/lib/security/jssecacerts
    # Comment previous line and uncomment next one for MacOs
    #sudo cp jssecacerts /Library/Java/Home/lib/security/


To use the script, it's required to have installed Java compiler and to download this file `InstallCert.java <http://code.google.com/p/java-use-examples/source/browse/trunk/src/com/aw/ad/util/InstallCert.java>`_, copying it in same path of script.


The script allow to import the certificate in the JVM keystore, providing the https server host and port and follow instructions::

    $ ./ssl_key_import.sh https_server_name 443

.. note :: Use previous script on your own risk. Before installing a certificate in the JVM keystore to be trusted, make sure you understand the security implications. 

.. note :: After importing the certificate it is required to restart GeoNetwork.



The main page
-------------

To access the harvesting main page you have to be logged in as an administrator.
From the administration page, click the link shown below with a red rectangle.

.. figure:: web-harvesting-where.png

    *How to access the harvesting main page*

The figure above shows the harvesting main page. The page shows a list of harvesting nodes that have been created so far. On the bottom side there is a set of buttons to manage these nodes. The meaning of each column is as follows:

#. *Select* This is just a check box to select one or more nodes. The selected nodes will be affected by the first row of buttons (start, stop, run, remove). For example, if you select 3 nodes and press the Remove button, these 3 nodes will be removed.
#. *Name* This is the node’s name provided by the administrator.
#. *Type* The node’s harvesting type chosen when the node was created (GeoNetwork, web folder etc...).
#. *Status* This is an icon that reflects the node’s current status. See :ref:`admin_harvesting_status` for all different icons and status description.
#. *Errors* This column reflects the status of the last harvesting run, which could have succeeded or not. The result is reflected on this icon and a tool tip will show detailed information.
#. *Every* The time (in days, hours, minutes) between two consecutive harvesting from this node.
#. *Last run* The date, in ISO 8601 format, of the most recent harvesting run.
#. *Operation* A list of buttons for all possible operations on a node.
#. Selecting *Edit* will allow you to change the parameters for a node.

.. figure:: web-harvesting-list.png

    *The harvesting main page*

The bottom side of the page contains two rows of buttons. The first row contains buttons
that can operate on a set of nodes. You can select the nodes using the check box on the first
column and then press the proper button. When the button finishes its action, the check boxes
are cleared. Here is the meaning of each button:

#.  *Activate* When a new harvesting node is created, it’s status is
    *inactive*. Use this button to make it
    *active* and thus to start harvesting from the remote node.

#.  *Deactivate* Stops harvesting from a node. Please notice that this does not mean that
    a currently running harvesting will be stopped but it means that this node will be
    ignored during future harvesting.

#.  *Run* This button simply tells the harvesting
    engine to start harvesting immediately. This is useful for testing during the
    harvesting setup.

#.  *Remove* Remove all currently selected nodes. A dialogue will ask the
    user to confirm the action.

The second row contains general purpose buttons. Here is the meaning of each button:

#.  *Back* Simply returns to the main administration page.

#.  *Add* This button allows to create new harvesting nodes.

#.  *Refresh* Refreshes the current list of nodes querying the server. This
    can be useful to see if the harvesting list has been altered by someone else or if
    any harvesting process started.

.. _admin_harvesting_status:

Harvesting Status and Error Icons
`````````````````````````````````

.. |fcl| image:: icons/fileclose.png
.. |clo| image:: icons/clock.png
.. |exe| image:: icons/exec.png


=====    ========    =======================================================
Icon     Status      Description
=====    ========    =======================================================
|fcl|    Inactive    The harvesting from this node is stopped.
|clo|    Active      The harvesting engine is waiting for the timeout 
                     specified for this node. When the timeout is reached, 
                     the harvesting starts.
|exe|    Running     The harvesting engine is currently running, fetching 
                     metadata from remote nodes. When the process will be 
                     finished, the status will be switched to active.
=====    ========    =======================================================

*Possible status icons*

.. |ok| image:: icons/button_ok.png
.. |imp| image:: icons/important.png

=====    ==============================================================
Icon     Description
=====    ==============================================================
|ok|     The harvesting was OK, no errors were found. In this case, a
         tool tip will show some harvesting results (like the number of
         harvested metadata etc...).
|imp|    The harvesting was aborted due to an unexpected condition. In
         this case, a tool tip will show some information about the
         error.
=====    ==============================================================

*Possible error icons*

Harvesting result tips
``````````````````````

If the harvesting succeeds, a tool tip will show detailed information about the
harvesting process. This way you can check if the harvester worked as expected
or if there is something to fix to the harvesting parameters or somewhere else.
The result tip is like a table, with rows labelled as follows:

- **Total** - This is the total number of metadata found remotely. Metadata with the same id are considered as one. 
- **Added** -  Number of metadata added to the system because they were not present locally. 
- **Removed** - Number of metadata that have been removed locally because they are not present in the remote server anymore.
- **Updated** - Number of metadata that are present locally but that needed to be updated because their last change date was different from the remote one.
- **Unchanged** - Local metadata left unchanged. Their remote last change date did not change. 
- **Unknown schema** - Number of skipped metadata because their format was not recognised by GeoNetwork. 
- **Unretrievable** - Number of metadata that were ready to be retrieved from the remote server but for some reason there was an exception during the data transfer process. 
- **Bad Format** - Number of skipped metadata because they did not have a valid XML representation. 
- **Does not validate** - Number of metadata which did not validate against their schema. These metadata were harvested with success but skipped due to the validation process. Usually, there is an option to force validation: if you want to harvest these metadata anyway, simply turn/leave it off.
- **Thumbnails/Thumbnails failed** - Number of metadata thumbnail images added/that could not be added due to some failure.
- **Metadata URL attribute used** - Number of layers/featuretypes/coverages that had a metadata URL that could be used to link to a metadata record (OGC Service Harvester only).
- **Services added** - Number of ISO19119 service records created and added to the catalogue (for THREDDS catalog harvesting only).
- **Collections added** - Number of collection dataset records added to the catalogue (for THREDDS catalog harvesting only).
- **Atomics added** - Number of atomic dataset records added to the catalogue (for THREDDS catalog harvesting only).
- **Subtemplates added** - Number of subtemplates (= fragment visible in the catalog) added to the metadata catalog.
- **Subtemplates removed** - Number of subtemplates (= fragment visible in the catalog) removed from the metadata catalog.
- **Fragments w/Unknown schema** - Number of fragments which have an unknown metadata schema.
- **Fragments returned** - Number of fragments returned by the harvester.
- **Fragments matched** - Number of fragments that had identifiers that in the template used by the harvester.
- **Existing datasets** - Number of metadata records for datasets that existed when the THREDDS harvester was run.
- **Records built** - Number of records built by the harvester from the template and fragments.
- **Could not insert** - Number of records that the harvester could not insert into the catalog (usually because the record was already present eg. in the Z3950 harvester this can occur if the same record is harvested from different servers).


==============================   ==========  ======     ======   =======  ===========  ================  =======  ===============
Result vs harvesting type        GeoNetwork  WebDAV     CSW      OAI-PMH  OGC Service  OGC WFS Features  THREDDS  Z3950 Server(s)
==============================   ==========  ======     ======   =======  ===========  ================  =======  ===============
Total                            |ok|        |ok|       |ok|     |ok|     |ok|         |ok|              |ok|     |ok|
Added                            |ok|        |ok|       |ok|     |ok|     |ok|                                    |ok|
Removed                          |ok|        |ok|       |ok|     |ok|     |ok|                                    |ok|
Updated                          |ok|        |ok|       |ok|     |ok|                                             |ok|
Unchanged                        |ok|        |ok|       |ok|     |ok|                                             |ok|
Unknown schema                   |ok|        |ok|       |ok|     |ok|     |ok|                                    |ok|
Unretrievable                    |ok|        |ok|       |ok|     |ok|     |ok|                           |ok|     |ok|
Bad Format                                   |ok|                |ok|                                             |ok|
Does Not Validate                            |ok|                |ok|                  |ok|                       |ok|
Thumbnails / Thumbnails failed                                            |ok|                           |ok|
Metadata URL attribute used                                               |ok|
Services Added                                                                                           |ok|
Collections Added                                                                                        |ok|
Atomics Added                                                                                            |ok|
Subtemplates Added                                                                     |ok|              |ok|
Subtemplates removed                                                                   |ok|              |ok|
Fragments w/Unknown Schema                                                             |ok|              |ok|
Fragments Returned                                                                     |ok|              |ok|
Fragments Matched                                                                      |ok|              |ok|
Existing datasets                                                                                        |ok|
Records Built                                                                          |ok|              |ok|
Could not insert                                                          |ok|                                    |ok|
==============================   ==========  ======     ======   =======  ===========  ================  =======  ===============


*Result information supported by harvesting types*

Adding new nodes
----------------

The Add button in the main page allows you to add new harvesting nodes. A drop down list is then shown with all the available harvester protocols. 

.. figure:: web-harvesting-add.png

    *Adding a new harvesting node*

You can choose the type of harvest you intend to perform and press *Add* to begin the process of adding the harvester. The supported harvesters and details of what to do next are in the following sections:

.. toctree::
		:maxdepth: 1

		gn/index.rst
		webdav/index.rst
		csw/index.rst
		gn2.0/index.rst
		oaipmh/index.rst
		ogcwxs/index.rst
		sde/index.rst
		thredds/index.rst
		wfs-getfeatures/index.rst
		z3950/index.rst


.. _harvest_history:

Harvest History
---------------

Each time a harvester is run, it generates a status report of what was harvested and/or what went wrong (eg. exception report). These reports are stored in a table in the database used by GeoNetwork. The entire harvesting history for all harvesters can be recalled using the History button on the Harvesting Management page. The harvest history for an individual harvester can also be recalled using the History link in the Operations for that harvester.


.. figure:: web-harvesting-with-history.png
		
		*An example of the Harvesting Management Page with History functions*

.. figure:: web-harvesting-all-harvesters-history.png
		
		*An example of the Harvesting History for all harvesters*

.. figure:: web-harvesting-harvester-history.png
		
		*An example of the Harvesting History for a harvester*

Once the harvest history has been displayed it is possible to:

- expand the detail of any exceptions
- sort the history by harvest date (or in the case of the history of all harvesters, by harvester name)
- delete any history entry or the entire history
