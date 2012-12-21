.. _webdav_harvester:

WEBDAV Harvesting
=================

This harvesting type uses the webDAV (Distributed Authoring and Versioning) protocol to harvest metadata from a DAV server. It can be useful to users that want to publish their metadata through a web server that offers a DAV interface. The protocol allows to retrieve the contents of a web page (a list of files) with their change date.

*Notes:*

#.  The same metadata could be harvested several times by different
    harvesting nodes. Anyway, this is not a good practise because every copy
    of the metadata will have a different UUID and the system will fill with
    different copies of the same metadata.

Adding a WebDAV node
````````````````````

In this type of harvesting, metadata are retrieved from a remote web page. The
available options are shown below and have the following meaning:

.. figure:: web-harvesting-webdav.png

    *Adding a WebDAV node*

- **Site** - This contains the connection information.

    - *Name* - This is a short description of the node. It will be shown in the harvesting main page. 
    - *URL* - The remote URL from which metadata will be harvested. Each file found that ends with .xml will indicate a metadata and will be retrieved, converted into XML and imported.
    - *Icon* - Just an icon to assign to harvested metadata. The icon will be used when showing search results. 
    - *Use account* - Account credentials for a basic HTTP authentication towards the remote URL. 

- **Options** - General harvesting options.

    - *Run at* - This is the harvesting start time. 
    - *Will run again every* - This option allows to select the schedule interval (each X hours from start time) and days to schedule the harvester.
    - *Validate* - If checked, the metadata will be validate during import. If the validation does not pass, the metadata will be skipped. 
    - *Recurse* - When the harvesting engine will find folders, it will recursively descend into them. 
- **Privileges** - Here it is possible to assign privileges to imported metadata. 

    - *Groups* - Groups area lists all available groups in GeoNetwork. Once one (or more) group has been selected, it can be added through the **Add** button (each group can be added only once). For each added group, a row of privileges is created at the bottom of the list to allow privilege selection. 
    - *Remove* - To remove a row simply press the associated Remove button on its right. 
    
- **Categories** - Here you can assign local categories to harvested metadata.
