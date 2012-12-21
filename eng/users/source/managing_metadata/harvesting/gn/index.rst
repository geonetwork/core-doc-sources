.. _harvesting_gn:

GeoNetwork Harvesting
---------------------

This is the standard and most powerful harvesting protocol used in GeoNetwork. It is able to log in into the remote node, to perform a standard search using the common query fields and to import all matching metadata. Furthermore, the protocol will try to keep both remote privileges and categories of the harvested metadata if they exist locally. Please notice that since GeoNetwork 2.1 the harvesting protocol has been improved. This means that it is not possible to use this protocol to harvest from version 2.0 or below.

Notes:

#. During harvesting, site icons are harvested and local copies updated. Icons are propagated to new nodes as soon as these nodes harvest from this one.
#. The metadata UUID is taken from the info.xml file of the MEF bundle. Any UUID stored inside the metadata will be overwritten with this one.

Adding a GeoNetwork node
````````````````````````

This type of harvesting allows you to connect to a GeoNetwork node, perform a
simple search as in the main page and retrieve all matched metadata. The search
is useful because it allows you to focus only on metadata of interest. Once you
add a node of this type, you will get a page like the one shown below. 

.. figure:: web-harvesting-gn.png

*Adding a GeoNetwork Node*

A description of the options follows:

- **Site** - Here you put information about the GeoNetwork’s node you want to harvest from. The url should have the format: ``http[s]://server[:port]/geonetwork``. If you want to search protected metadata you have to specify an account. The name parameter is just a short description that will be shown in the main page beside each node. 

- **Site** - Here you put information about the GeoNetwork’s node you want to harvest from (host, port and servlet). If you want to search protected metadata you have to specify an account. The name parameter is just a short description that will be shown in the main page beside each node. 

- **Set categories if exist locally** - This option allows to propagate categories from one node to another for existing 
  categories in the local node.

- **Use full MEF format** - This option will use the MEF format including document (eg. thumbnails) to retrieve the remote records.

- **XSL filter name** - This option will apply a custom XSL filter before the record is inserted in local node. A common use case is 
  to anoymize metadata records using the anonymizer process which remove or rename contact personal information (See the :ref:`processing` 
  section for more information).


- **Search criteria** - In this section you can specify search parameters: they are the same present in the GeoNetwork homepage. Before doing that, it is important to remember that the GeoNetwork’s harvesting can be hierarchical so a remote node can contain both its metadata and metadata harvested from other nodes and sources. At the beginning, the Source drop down is empty and you have to use the **Retrieve sources** button to fill it. The purpose of this button is to query GeoNetwork about all sources which it is currently harvesting from. Once you get the drop down filled, you can choose a source name to constrain the search to that source only. Leaving the drop down blank, the search will spread over all metadata (harvested and not). You can add several search criteria for each site through the **Add** button: several searches will be performed and results merged. Each search box can be removed pressing the small button on the left of the site’s name. If no search criteria is added, a global unconstrained search will be performed. 

- **Options** - This is just a container for general options.

    - *Run at* - This is the harvesting start time. 
    - *Will run again every* - This option allows to select the schedule interval (each X hours from start time) and days to schedule the harvester.
    - *One run only* - If this option is checked, the harvesting will do only one run after which it will become inactive. 
    
- **Privileges** - Here you decide how to map remote group’s privileges. You can assign a copy policy to each group. The Intranet group is not considered because it does not make sense to copy its privileges. 

    - The **All** group has different policies from all the others:

        #.  Copy: Privileges are copied.
        #.  Copy to Intranet: Privileges are copied but to the Intranet group.
            This way public metadata can be made protected.
        #.  Don’t copy: Privileges are not copied and harvested metadata will not
            be publicly visible.

    - For all other groups the policies are these:

        #.  Copy: Privileges are copied only if there is a local group with the
            same (not localised) name as the remote group.
        #.  Create and copy: Privileges are copied. If there is no local group
            with the same name as the remote group then it is created.
        #.  Don’t copy: Privileges are not copied.
