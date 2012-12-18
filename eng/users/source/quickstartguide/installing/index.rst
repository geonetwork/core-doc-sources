.. _installing:

Installing the software
=======================

New version - New functionality
------------------------------

The new GeoNetwork opensource comes with substantial upgrades of different components. 

- **Javascipt widget user interface:** A new user interface using one of the latest Javascript widget libraries (extJS) has been added to support searching, editing and viewing metadata records. The user interface is now much easier for Javascript developers to reorganize and customize. GeoNetwork comes with two flavours of home page: one has the sidebar search similar to the old interface and the other uses a tabbed search layout. The 2.6.x user interface is still available.
- **Search Statistics:** Captures and displays statistics on searches carried out in GeoNetwork. The statistics can be summarized in tables or in charts using JFreeChart. There is an extensible interface that you can use to display your own statistics.
- **Metadata Status:** Allows finer control of the metadata workflow. Records can be assigned a status that reflects where they are in the metadata workflow: draft, approved, retired, submitted, rejected. When the status changes the relevant user is informed via email. eg. when an editor changes the status to 'submitted', the content reviewer receives an email requesting review.
- **Metadata Versioning:** Captures changes to metadata records and metadata properties (status, privileges, categories) and records them as versions in a subversion respository.
- **Publishing data to GeoServer from GeoNetwork:** You can now publish geospatial information in the form of GeoTIFF, shapefile or spatial table in a database to GeoServer from GeoNetwork. See :ref:`GeoPublisher`.
- **Custom Metadata Formatters:** You can now create your own XSLT to format metadata to suit your needs, zip it up and plug it in to GeoNetwork.
- **Assembling Metadata Records from Reusable Components:** Metadata records can now be assembled from reusable components (eg. contact information). The components can be present in the local catalog or brought in from a remote catalog (with caching to speed up access). A component directory interface is available for editing and viewing the components.
- **Editor Improvements:** Picking terms from a thesaurus using a search widget, selecting reusable metadata components for inclusion in the record, user defined suggestions or picklists to control content, context sensitive help, creating relationships between records.
- **New Harvesters:** OGC Harvesting: Sensor Observation Service, Z3950 harvesting, Web Accessible Folder (WAF)
- **Harvest History and Scheduling:** Harvesting events are now recorded in the database for review at any time. Harvester scheduling is now much more flexible, you can start a harvest at any time of the day and at almost any interval (weekly etc).
- **Extended Metadata Exchange Format (MEF):** More than one metadata file can be present in a MEF Zip archive. This is MEF version 2.
- **Plug in metadata schemas:** You can define your own metadata schema and plug it into GeoNetwork on demand. Documentation to help you do this and example plug in schemas can be found in the Developers Manual. Some of the most common community plug in schemas can be downloaded from the GeoNetwork source code repository.
- **Improved Database Connection Handling and Pooling:** Replacement of the Jeeves based database connection pool with the widely used and more robust Apache Database Connection Pool (DBCP). Addition of JNDI or container based database connection support. See :ref:`Database_JNDI_configuration`.
- **Support for the INSPIRE Directive:** Indexing and user interface extensions to support those who need to implement the INSPIRE metadata directive (EU).
- **Virtual CSW Endpoints:** Now you can define a custom CSW service that works with a set of metadata records that you define. See :ref:`VirtualCSW`.
- **Configuration Overrides:** Now you can add your own configuration options to GeoNetwork, keep them in one file and maintain them independently from GeoNetwork. See :ref:`adv_configuration_overriddes`.
- **Multilingual Indexing:** If you have to cope with metadata in different languages, GeoNetwork can now index each language and search all across language indexes by translating your search terms. See :ref:`multilingual`.
- **Many other improvements:** charset detection and conversion on import, batch application of an XSLT to a selected set of metadata records, remote notification of metadata changes, automatic integration tests to improve development and reduce regression and, of course, many bug fixes.

.. figure:: Home_page_tn.png

    *New home page of GeoNetwork opensource using JavaScript Widgets - tab layout*

.. figure:: Home_page_n.png

    *New home page of GeoNetwork opensource using JavaScript Widgets- sidebar layout*


Where do I get the installer?
-----------------------------

The software is distributed through the SourceForge.net Website at http://sourceforge.net/projects/geonetwork.

Use the platform independent installer (.jar) for all platforms except Windows. Windows has a .exe file installer.

System requirements
-------------------

GeoNetwork can run either on **MS Windows** , **Linux** or **Mac OS X** .

Some general system requirements for the software to run without problems are listed below:

**Processor** : 1 GHz or higher

**Memory (RAM)** : 1 GB or higher

**Disk Space** : Minimum of 512MB of free disk space. Additional space is required depending on the amount of spatial data that you expect to upload.

**Other Software requirements** : A Java Runtime Environment (JRE 1.6.0). For server installations, Apache Tomcat and a dedicated JDBC compliant DBMS (MySQL, Postgresql, Oracle) can be used instead of Jetty and H2.

Additional Software
```````````````````

The software listed here is not required to run GeoNetwork, but can be used for custom installations.

#. MySQL DBMS v5.5+ (All) [#all_os]_
#. Postgresql DBMS v7+ (All) [#all_os]_
#. Apache Tomcat v5.5+ (All) [#all_os]_

Supported browsers
``````````````````

GeoNetwork should work normally with the following browsers:

#. Firefox v1.5+ (All) [#all_os]_
#. Internet Explorer v8+ (Windows)
#. Safari v3+ (Mac OS X Leopard)

How do I install GeoNetwork opensource?
---------------------------------------

Before running the GeoNetwork installer, make sure that all system requirements are satisfied, and in particular that the Java Runtime Environment version 1.6.0 is set up on your machine.

On Windows
``````````

If you use Windows, the following steps will guide you to complete the installation (other FOSS will follow):

1. Double click on **geonetwork-install-2.8.0.exe** to start the GeoNetwork opensource desktop installer
2. Follow the instructions on screen. You can choose to install the embedded map server (based on `GeoServer <http://www.geoserver.org>`_, GAST and the European Union Inspire Directive configuration pack. Developers may be interested in installing the source code and installer building tools. Full source code can be found in the GeoNetwork github code repository at http://github.com/geonetwork.
3. After completion of the installation process, a 'GeoNetwork desktop' menu will be added to your Windows Start menu under 'Programs'
4. Click Start\>Programs\>GeoNetwork desktop\>Start server to start the Geonetwork opensource Web server. The first time you do this, the system will require about 1 minute to complete startup.
5. Click Start\>Programs\>Geonetwork desktop\>Open GeoNetwork opensource to start using GeoNetwork opensource, or connect your Web browser to `http://localhost:8080/geonetwork/ <http://localhost:8080/geonetwork/>`_

.. figure:: installer.png

   *Installer*

.. figure:: install_packages.png

   *Packages to be installed*

The installer allows to install these additional packages:

1. GeoNetwork User Interface: Experimental UI for GeoNetwork using javascript components based on ExtJs library.
2. GeoServer: Web Map Server that provides default base layers for the GeoNetwork map viewer.
3. European Union INSPIRE Directive configuration pack: Enables INSPIRE support in GeoNetwork.
 - INSPIRE validation rules.
 - Thesaurus files (GEMET, Inspire themes).
 - INSPIRE search panel.
 - INSPIRE metadata view.
4. GAST: Installs GeoNetwork's Administrator Survival Tool. See :ref:`gast`.

Installation using the platform independent installer
`````````````````````````````````````````````````````

If you downloaded the platform independent installer (a .jar file), you can in most cases start the installer by simply double clicking on it.

Follow the instructions on screen (see also the section called On Windows).

At the end of the installation process you can choose to save the installation script (Figure Save the installation script for commandline installations).

.. figure:: install_script.png
   
   *Save the installation script for commandline installations*


Commandline installation
````````````````````````

If you downloaded the platform independent installer (a .jar file), you can perform commandline installations on computers without a graphical interface. You first need to generate an install script (see Figure Save the installation script for commandline installations). This install script can be edited in a text editor to change some installation parameters.

To run the installation from the commandline, issue the following command in a terminal window and hit enter to start::

    java -jar geonetwork-install-2.8.0.jar install.xml
    [ Starting automated installation ]
    Read pack list from xml definition.
    Try to add to selection [Name: Core and Index: 0]
    Try to add to selection [Name: GeoServer and Index: 1]
    Try to add to selection [Name: European Union INSPIRE Directive configuration pack and Index: 2]
    Try to add to selection [Name: GAST and Index: 3]
    Modify pack selection.
    Pack [Name: European Union INSPIRE Directive configuration pack and Index: 2] added to selection.
    Pack [Name: GAST and Index: 3] added to selection.
    [ Starting to unpack ]
    [ Processing package: Core (1/4) ]
    [ Processing package: GeoServer (2/4) ]
    [ Processing package: European Union INSPIRE Directive configuration pack (3/4) ]
    [ Processing package: GAST (4/4) ]
    [ Unpacking finished ]
    [ Creating shortcuts ....... done. ]
    [ Add shortcuts to uninstaller  done. ]
    [ Writing the uninstaller data ... ]
    [ Automated installation done ]

You can also run the installation with lots of debug output. To do so run the installer with the flag *-DTRACE=true*::

  java -DTRACE=true -jar geonetwork-install-2.8.0.jar

.. [#all_os] All = Windows, Linux and Mac OS X


User interface configuration
----------------------------

As mentioned above, GeoNetwork now provides two user interfaces: 

- **Default** user interface is the old user interface from 2.6.x and earlier
- **Javascript Widgets** user interface is the new user interface for searching, editing and viewing metadata records in 2.8.x

The catalog administrator can configure which interface to use in `WEB-INF/config-gui.xml` as follows. 


Configuring the Default user interface
``````````````````````````````````````

`WEB-INF/config-gui.xml` is used to define which home page to use. To configure the Default user interface use::

    <client type="redirect" 
      widget="false" 
      url="main.home"
      parameters=""
      stateId=""
      createParameter=""/>
  

Configuring the Javascript Widgets user interface
`````````````````````````````````````````````````

Widgets can be used to build custom interfaces. GeoNetwork provides a Javascript Widgets interface for searching, viewing and editing metadata records.


This interface can be configured using the following attributes:

 - **parameter** is used to define custom application properties like default map extent for example or change the default language to be loaded

 - **createParameter** is appended to URL when the application is called from the administration > New metadata menu (usually "#create").

 - **stateId** is the identifier of the search form (usually "s") in the application. It is used to build quick links section in the administration and permalinks.


Sample configuration::

  <!-- Widget client application with a tab based layout -->
  <client type="redirect" 
    widget="true" 
    url="../../apps/tabsearch/" 
    createParameter="#create" 
    stateId="s"/>
    


Configuring the user interface with configuration overrides
```````````````````````````````````````````````````````````

Instead of changing config-gui.xml file, the catalog administrator could use the configuration overrides mechanism to create a custom configuration (See :ref:`adv_configuration_overriddes`). By default, no overrides are set and the Default user interface is loaded. 

To configure which user interface to load, add the following line in WEB-INF/config-overrides.xml in order to load
the Widgets based user interface::
 
 
    <override>/WEB-INF/config-overrides-widgettab.xml</override>



XSLT processor configuration
----------------------------

The file ``INSTALL_DIR/web/geonetwork/WEB-INF/classes/META-INF/javax.xml.transform.TransformerFactory`` defines the XSLT processor to use in GeoNetwork. The allowed values are:

#. ``de.fzi.dbs.xml.transform.CachingTransformerFactory``: This is the Saxon XSLT processor with caching (recommended value for production use). However, when caching is on, any updates you make to stylesheets may be ignored in favour of the cached stylesheets.
#. ``net.sf.saxon.TransformerFactoryImpl``: This is the Saxon XSLT processor *without* caching. If you plan to make changes to any XSLT stylesheets you should use this setting until you are ready to move to production.

GeoNetwork sets the XSLT processor configuration using Java system properties for an instant in order to obtain its TransformerFactory implementation, then resets it to the original value, to minimize affect the XSL processor configuration for other applications that may be running in the same container.

Database configuration
----------------------

Geonetwork uses the `H2 database engine <http://www.h2database.com/>`_ as default. The following additional database backends are supported (listed in alphabetical order):

* DB2
* H2
* Mckoi
* MS SqlServer 2008
* MySQL
* Oracle
* PostgreSQL (or PostGIS)


Configure config.xml
````````````````````

The database backend used is configured in **INSTALL_DIR/WEB-INF/config.xml**. The following xml element is of interest::


                <!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
                <!-- H2 database  http://www.h2database.com/ -->
                <!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
                <resource enabled="true">
                 <name>main-db</name>
                 <provider>jeeves.resources.dbms.ApacheDBCPool</provider>
                 <config>
                   <user>admin</user>
                   <password>gnos</password>
                   <driver>org.h2.Driver</driver>
                   <url>jdbc:h2:geonetwork;MVCC=TRUE</url>
                   <poolSize>33</poolSize>
                   <reconnectTime>3600</reconnectTime>
                 </config>
                </resource>

The attribute enabled has to be changed from **true** to **false** ::

                <!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
                <!-- H2 database  http://www.h2database.com/ -->
                <!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
                <resource enabled="false">
                    ...
                </resource>


The resource element for the required database must be enabled. If two resources are enabled, GeoNetwork will fail to start. 
At a minimum, **<user>** , **<password>** and **<url>** have to be changed. (Showing DB2 as an example without loss of generality)::

               <resource enabled="true">
                        <name>main-db</name>
                        <!-- <provider>jeeves.resources.dbms.DbmsPool</provider> -->
                        <provider>jeeves.resources.dbms.ApacheDBCPool</provider>
                        <config>
                                <user>db2inst1</user>
                                <password>mypassword</password>
                                <driver>com.ibm.db2.jcc.DB2Driver</driver>
                                <url>jdbc:db2:geonet</url>
                                <poolSize>10</poolSize>
                        </config>
                </resource>



Connection Pool
```````````````

GeoNetwork support two types of database connection pool:

* `Apache DBCP pool <http://commons.apache.org/dbcp/>`_ This pool is recommended for smaller catalogs (less than 10,000 records).
* `JNDI pool` - configure the database connection pool in Jetty or Tomcat. It is recommended for larger catalogs (especially those with more than approx 30,000 records).

More details about the JNDI pool and the configuration parameters that can be used here are in the advanced configuration section of this manual (See :ref:`Database_JNDI_configuration`).


JDBC Drivers
````````````
For the Apache DBCP pool, JDBC database driver jar files should be in **INSTALL_DIR/WEB-INF/lib**.  For Open Source databases, like MySQL and PostgreSQL, the jar files are already installed. For commercial databases like Oracle, the jar files must be downloaded and installed manually. This is due to licensing issues.

* `DB2 JDBC driver download <https://www-304.ibm.com/support/docview.wss?rs=4020&uid=swg27016878>`_
* `MS Sql Server JDBC driver download <http://msdn.microsoft.com/en-us/sqlserver/aa937724>`_
* `Oracle JDBC driver download <http://www.oracle.com/technetwork/database/features/jdbc/index-091264.html>`_


Creating and initializing tables
````````````````````````````````

At startup, GeoNetwork checks if the database tables it needs are present in the database.  If not, the tables are created and filled with initial data. 

If the database tables are present but were created with an earlier version of GeoNetwork, then a migration script is run.

An alternative to running these scripts automatically is to execute them manually. This is preferable for those that would like to examine and monitor the changes being made to their database tables.

* The scripts for initial setup are located in **INSTALL_DIR/WEB-INF/classes/setup/sql/create/**
* The scripts for inserting initial data are located in **INSTALL_DIR/WEB-INF/classes/setup/sql/data/**
* The scripts for migrating are located in **INSTALL_DIR/WEB-INF/classes/setup/sql/migrate/**

An example for a manual DB2 setup::

        db2 create db geonet
        db2 connect to geonet user db2inst1 using mypassword
        db2 -tf INSTALL_DIR/WEB-INF/classes/setup/sql/create/create-db-db2.sql > res1.txt
        db2 -tf INSTALL_DIR/WEB-INF/classes/setup/sql/data/data-db-default.sql > res2.txt
        db2 connect reset

After execution, check **res1.txt** and **res2.txt** if errors have occurred.

.. note::

    Known DB2 problem. DB2 may produce an exception during first time geonetwork start.

        DB2 SQL error: SQLCODE: -805, SQLSTATE: 51002, SQLERRMC: NULLID.SYSLH203

    Solution one is to setup the database manually a described in the previous chapter.
    Solution two is to drop the database, recreate it,locate the file db2cli.lst in the db2 installation folder and execute

        db2 bind @db2cli.lst CLIPKG 30


Upgrading to a new Version
==========================

The upgrade process from one version to another is typically a fairly simple process.  Following the normal setup instructions, should result in Geonetwork successfully upgrading the internal datastructures from the old version to the new version.  The exceptions to this rule are:

* Migration to Geonetwork 2.8 will reset all harvesters to run every 2 hours. This is because the underlying harvester scheduler has been changed and the old schedules are not longer supported.  In this case one must review all the harvesters and define new schedules for them.
