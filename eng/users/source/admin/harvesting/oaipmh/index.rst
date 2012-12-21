.. _oaipmh_harvester:

OAIPMH Harvesting
=================

This is a harvesting protocol that is widely used among libraries. GeoNetwork implements version 2.0 of the protocol.
        
Notes:

#.  The id of the remote server must be a UUID. If not, metadata can be
    harvested but during hierarchical propagation id clashes could corrupt
    harvested metadata.
#.  During harvesting, GeoNetwork will try to auto detect the schema of
    each metadata. If the schema is not supported the metadata is
    skipped.

Adding an OAI-PMH node
``````````````````````

An OAI-PMH server implements a harvesting protocol that GeoNetwork, acting as
a client, can use to harvest metadata. If you are requesting the oai_dc output
format, GeoNetwork will convert it into its Dublin Core format. Other formats
can be harvested only if GeoNetwork supports them and is able to autodetect the
schema from the metadata.

.. figure:: web-harvesting-oaipmh.png

    *Adding an OAI-PMH harvesting node*

Configuration options:

- **Site** - All options are the same as WebDAV harvesting. The only difference is that the URL parameter here points to an OAI-PMH server. This is the entry point that GeoNetwork will use to issue all PMH commands. 
- **Search criteria** - This part allows you to restrict the harvesting to specific metadata subsets. You can specify several searches: GeoNetwork will execute them sequentially and results will be merged to avoid the harvesting of the same metadata. Several searches allow you to specify different search criteria. In each search, you can specify the following parameters:

    - *From* - You can provide a start date here. All metadata whose last change date is equal to or greater than this date will be harvested. You cannot simply edit this field but you have to use the icon to popup a calendar and choose the date. This field is optional so if you don’t provide it the start date constraint is dropped. Use the icon to clear the field. 
    - *Until* - Works exactly as the from parameter but adds an end constraint to the last change date. The until date is included in the date range, the check is: less than or equal to. 
    - *Set* - An OAI-PMH server classifies its metadata into hierarchical sets. You can request to return metadata that belong to only one set (and its subsets). This narrows the search result. Initially the drop down shows only a blank option that indicate *no set*. After specifying the connection URL, you can press the **Retrieve Info** button, whose purpose is to connect to the remote node, retrieve all supported sets and prefixes and fill the search drop downs. After you have pressed this button, you can select a remote set from the drop down. 
    - *Prefix* - Here prefix means metadata format. The oai_dc prefix is mandatory for any OAI-PMH compliant server, so this entry is always present into the prefix drop down. To have this drop down filled with all prefixes supported by the remote server, you have to enter a valid URL and press the Retrieve Info button.
    - You can use the Add button to add one more search to the list. A search can be removed clicking the icon on its left. 
    
- **Options** - Most options are equivalent to WebDAV harvesting. 

    - *Validate* - The validate option, when checked, will validate each harvested metadata against GeoNetwork’s schemas. Only valid metadata will be harvested. Invalid one will be skipped. 
    
- **Privileges** - Same as for WebDAV harvesting. 
- **Categories** - Same as for WebDAV harvesting.

Please note that when you edit a previously created node, both the *set* and *prefix* drop down lists will be empty. They will contain only the previously selected entries, plus the default ones if they were not selected. Furthermore, the set name will not be localised but the internal code string will be displayed. You have to press the retrieve info button again to connect to the remote server and retrieve the localised name and all set and prefix information.
