.. _metadata_xml_relations:

Metadata Relation services
==========================

Introduction
------------

This section describes the services used to get, set and delete relations between
metadata records in GeoNetwork. The relations are stored in the Relations
table as follows:

==========  ============================    ====================================
Field       Datatype                        Description
==========  ============================    ====================================
id          foreign key to Metadata(id)     Source metadata 
relatedId   foreign key to Metadata(id)     Metadata related to the source
==========  ============================    ====================================

xml.relation.get
----------------

This service retrieves all the related records for a source metadata record specified by id in the parameters.

Request
```````

- **id (integer)**: This is the local GeoNetwork
  identifier of the metadata whose relations are requested.

- **relation (string, ’normal’)**: This optional
  parameter identifies the kind of relation that the client wants to
  be returned. It can be one of these values:

  - **normal**: The service performs a query into the id field
    and returns all relatedId records.
  - **reverse**: The service performs a query into the relatedId
    field and returns all id records.
  - **full**: Includes both normal and reverse queries
    (duplicated ids are removed).

Here is an example of POST/XML request::

    <request>
        <id>10</id>
        <relation>full</relation>
    </request>

Response
````````

The response has a response root element with several metadata children
depending on the relations found. Example::

    <response>
        <metadata>...</metadata>
        <metadata>...</metadata>
        ...
    </response>

Each metadata element has the following structure:

- **title**: Metadata title

- **abstract**: A brief explanation of the metadata

- **keyword**: Keywords found inside the metadata

- **image**: Information about thumbnails

- **link**: A link to the source site

- **geoBox**: coordinates of the bounding box

- **geonet:info**: A container for GeoNetwork related
  information

Example of a metadata record::

    <metadata>
        <title>Globally threatened species of the world</title>
        <abstract> Contains information on animals.</abstract>
        <keyword>biodiversity</keyword>
        <keyword>endangered animal species</keyword>
        <keyword>endangered plant species</keyword>
        <link type="url">http://www.mysite.org</link>
        <geoBox>
            <westBL>-180.0</westBL>
            <eastBL>180.0</eastBL>
            <southBL>-90.0</southBL>
            <northBL>90.0</northBL>
        </geoBox>
        <geonet:info>
            <id>11</id>
            <schema>fgdc-std</schema>
            <createDate>2005-03-31T19:13:31</createDate>
            <changeDate>2007-03-12T14:52:46</changeDate>
            <isTemplate>n</isTemplate>
            <title/>
            <source>38b75c1b-634b-443e-9c36-a12e89b4c866</source>
            <UUID>84b4190b-de43-4bd7-b25f-6ed47eb239ac</uuid>
            <isHarvested>n</isHarvested>
            <view>true</view>
            <admin>false</admin>
            <edit>false</edit>
            <notify>false</notify>
            <download>true</download>
            <dynamic>false</dynamic>
            <featured>false</featured>
        </geonet:info>
    </metadata>

