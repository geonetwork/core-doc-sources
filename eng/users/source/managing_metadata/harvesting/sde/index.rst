.. _sde_harvester:

Harvesting an ARCSDE Node
=========================

This is a harvesting protocol for metadata stored in an ArcSDE installation.

Adding an ArcSDE server
```````````````````````

The ArcSDE harvester allows harvesting metadata from an ArcSDE installation. ArcSDE java API libraries are required to be installed by the user in GeoNetwork (folder ``INSTALL_DIR/web/geonetwork/WEB-INF/lib``), as these are proprietary libraries not distributed with GeoNetwork: 
	
	- jpe_sdk.jar
	- jsde_sdk.jar
	
.. note :: dummy-api-XXX.jar must be removed from ``INSTALL_DIR/web/geonetwork/WEB-INF/lib``

The harvester identifies the ESRI metadata format: ESRI ISO, ESRI FGDC to apply the required xslts to transform metadata to ISO19139

.. figure:: web-harvesting-sde.png

    *Adding an ArcSDE harvesting node*

Configuration options:

- **Site** 

	- *Name* - This is a short description of the node. It will be shown in the harvesting main page.  
	- *Server* - ArcSde server IP or name
	- *Port* - ArcSde service port (typically 5151)
	- *Username* - Username to connect to ArcSDE server
	- *Password* - Password of the ArcSDE user
	- *Database name* - ArcSDE instance name (typically esri_sde)

- **Options** - Same as for WebDAV harvesting. 
- **Privileges** - Same as for WebDAV harvesting. 
- **Category for service** - Metadata for the harvested service is linked to the category selected for the service (usually "interactive resources").
- **Category for datasets** - For each dataset, the "category for datasets" is linked to each metadata for datasets.
