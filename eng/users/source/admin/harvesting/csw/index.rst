.. _csw_harvester:

CSW Harvesting
--------------

Adding a CSW node
`````````````````

This type of harvesting is capable of connecting to a remote CSW server and
retrieving all matching metadata. Please, note that in order to be harvested
metadata must have one of the schema format handled by GeoNetwork.

.. figure:: web-harvesting-csw.png

    *Adding a Catalogue Services for the Web harvesting node*

The figure above shows the options available:

- **Site** - Here you have to specify the connection parameters which are similar to the web DAV harvesting. In this case the URL points to the capabilities document of the CSW server. This document is used to discover the location of the services to call to query and retrieve metadata. 
- **Search criteria** - Using the Add button, you can add several search criteria. You can query only the fields recognised by the CSW protocol. 
- **Options** - General harvesting options:

    - *Run at* - This is the harvesting start time. 
    - *Will run again every* - This option allows to select the schedule interval (each X hours from start time) and days to schedule the harvester.
    - *One run only* - If this option is checked, the harvesting will do only one run after which it will become inactive. 
    
- **Privileges** - Same as for WebDAV harvesting.
- **Categories** - Same as for WebDAV harvesting.
