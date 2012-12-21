.. _ogcwxs_harvester:

Harvesting OGC Services
=======================

*Notes:*

#.  Every time the harvester runs, it will remove previously harvested information and create new ones. GeoNetwork will generate the id for all metadata (both service and datasets).  Therefore, for datasets, if the metadata is created using a remote XML document (ie. if a MetadataUrl tag is in the GetCapability document), the UUID of the document is used.
#.  Thumbnails are generated only for Web Map Service (WMS). The service should also support the WGS84 projection.

Adding an OGC Service Harvester (ie. WMS, WFS, WCS)
```````````````````````````````````````````````````

An OGC service implements a GetCapabilities operation that GeoNetwork, acting as
a client, can use to produce metadata. The GetCapability document provides information about the
service and the layers/feature types/coverages served. GeoNetwork will convert it into
ISO19139/119 format.

.. figure:: web-harvesting-ogc.png

    *Adding an OGC service harvesting node*

Configuration options:

- **Site** 

    - *Name* - The name of the catalogue and will be one of the search criteria. 
    - *Type* - The type of OGC service indicates if the harvester has to query for a specific kind of service. Supported type are WMS (1.0.0 and 1.1.1), WFS (1.0.0 and 1.1.0, WCS (1.0.0) and WPS (0.4.0 and 1.0.0). 
    - *Service URL* - The service URL is the URL of the service to contact (without parameters like "REQUEST=GetCapabilities", "VERSION=", ...). It has to be a valid URL like http://your.preferred.ogcservice/type_wms. 
    - *Metadata language* - Required field that will define the language of the metadata. It should be the language used by the web service administrator.
    - *ISO topic category* - Used to populate the metadata. It is recommended to choose on as the topic is mandatory for the ISO standard if the hierarchical level is "datasets".
    - *Type of import* - Defines if the harvester should only produce one service metadata record or if it should loop over datasets served by the service and produce also metadata for each datasets. For each dataset the second checkbox allow to generate metadata for the dataset using an XML document referenced in the MetadataUrl attribute of the dataset in the GetCapability document. If this document is loaded but it is not valid (ie. unknown schema, bad XML format), the GetCapability document is used.

    For WMS, thumbnails could be created during harvesting.
    - *Icon* - The default icon displayed as attribution logo for metadata created by this harvester.
    
- **Options** - Same as for WebDAV harvesting. 
- **Privileges** - Same as for WebDAV harvesting. 
- **Category for service** - Metadata for the harvested service is linked to the category selected for the service (usually "interactive resources").
- **Category for datasets** - For each dataset, the "category for datasets" is linked to each metadata for datasets.
