.. _services_general:

General services
================

.. _xml.info:

xml.info
--------

The xml.info service can be used to query the site about its configuration,
services, status, schemas, users, groups and so on. 

Request
```````

The XML request should contain at least one type parameter to indicate the
kind of information to retrieve. Multiple type parameters can be specified.
The set of allowed values is:

- **site**: Returns general information about the site like its name, id, etc...

- **categories**: Returns all site’s categories

- **groups**: Returns all site’s groups visible to the requesting user. If the user does not authenticate himself, only the Intranet and the all groups are visible.

- **users**: Depending upon the profile of the user making the call, information about users of the site will be returned. The rules are:
 
 - Administrators can see all users
 - User administrators can see all users they administer and
   all other user administrators in the same group set. The group set
   is defined by all groups visible to the user administrator (except for
   the All and Intranet groups).
 - An authenticated user can only see their own information.
 - A guest cannot see any user information at all.

- **operations**: Returns all possible operations on metadata

- **regions**: Returns all geographical regions usable for queries

- **sources**: Returns all GeoNetwork sources (remote sites) that are known about at the site. This will include:

 - Node name and siteId
 - All source UUIDs and site names that have been discovered through harvesting
 - All source UUIDs and site names from MEF files imported by the site

- **schemas**: All registered metadata schemas for the site

- **status**: All possible status values for metadata records


Request example::

    <request>
        <type>site</type>
        <type>groups</type>
    </request>

Response
````````

Each type parameter produces an XML subtree in an info container element. An example response to a request for three types of information might look like the following::

    <info>
        <site>...</site>
        <categories>...</categories>
        <groups>...</groups>
        ...
    </info>

The structure of each possible subtree is as follows:

Site
^^^^

- **site**: This is the container

  - **name**: Human readable site name
  - **siteId**: Universal unique identifier of the site
  - **platform**: This is just a container to hold the site’s
    back end

    - **name**: Platform name. For GeoNetwork installations
      it must be GeoNetwork.
    - **version**: Platform version, given in the X.Y.Z
      format
    - **subVersion**: Additional version notes, like
      ’alpha-1’ or ’beta-2’.
      
Example site information::
  
      <site>
          <name>My site</name>
          <organisation>FAO</organization>
          <siteId>0619cc50-708b-11da-8202-000d9335906e</siteId>
          <platform>
              <name>geonetwork</name>
              <version>2.2.0</version>
          </platform>
      </site>

Categories
^^^^^^^^^^

- **categories**: This is the container for categories.

  - **category \[0..n]**: A single GeoNetwork’s category. This
    element has an id attribute which represents the local
    identifier for the category. It can be useful to a client
    to link back to this category.

    - **name**: Category’s name
    - **label**: The localised labels used to show the
      category on screen. See :ref:`xml_response_categories`.

Example response for categories::
  
      <categories>
          <category id="1">
              <name>datasets</name>
              <label>
                  <en>Datasets</en>
                  <fr>Jeux de données</fr>
              </label>
          </category>
      </categories>

Groups
^^^^^^

- **groups**: This is the container for groups

  - **group \[2..n]**: This is a GeoNetwork group. There are at
    least the Internet and Intranet groups. This element has an
    id attribute which represents the local identifier for the
    group.

    - **name**: Group’s name
    - **description**: Group’s description
    - **referrer**: The user responsible for this group
    - **email**: The email address to notify when a map is
      downloaded
    - **label**: The localised labels used to show the
      group on screen. See :ref:`xml_response_groups`.

Example response for groups::
  
      <groups>
          <group id="1">
              <name>editors</name>
              <label>
                  <en>Editors</en>
                  <fr>Éditeurs</fr>
              </label>
          </group>
      </groups>

Operations
^^^^^^^^^^

- **operations**: This is the container for the operations

  - **operation \[0..n]**: This is a possible operation on
    metadata. This element has an id attribute which represents
    the local identifier for the operation.

    - **name**: Short name for the operation.
    - **reserved**: Can be y or n and is used to
      distinguish between system reserved and user defined
      operations.
    - **label**: The localised labels used to show the
      operation on screen. See :ref:`xml_response_operations`.

Example response for operations::
  
      <operations>
          <operation id="0">
              <name>view</name>
              <label>
                  <en>View</en>
                  <fr>Voir</fr>
              </label>
          </operation>
      </operations>

Regions
^^^^^^^

- **regions**: This is the container for geographical regions

  - **region \[0..n]**: This is a region present into the system.
    This element has an id attribute which represents the local
    identifier for the operation.

    - **north**: North coordinate of the bounding box.
    - **south**: South coordinate of the bounding box.
    - **west**: West coordinate of the bounding box.
    - **east**: east coordinate of the bounding box.
    - **label**: The localised labels used to show the
      region on screen. See :ref:`xml_response_regions`.

Example response for regions::
  
      <regions>
          <region id="303">
              <north>82.99</north>
              <south>26.92</south>
              <west>-37.32</west>
              <east>39.24</east>
              <label>
                  <en>Western Europe</en>
                  <fr>Western Europe</fr>
              </label>
          </region>
      </regions>

Sources
^^^^^^^

- **sources**: This is the container.

  - **source \[0..n]**: A source known to the remote node.

    - **name**: Source’s name
    - **UUID**: Source’s unique identifier

Example response for a source::
  
      <sources>
          <source>
              <name>My Host</name>
              <UUID>0619cc50-708b-11da-8202-000d9335906e</uuid>
          </source>
      </sources>

Users
^^^^^

- **users**: This is the container for user information

  - **user \[0..n]**: A user of the system

    - **id**: The local identifier of the user
    - **username**: The login name
    - **surname**: The user’s surname. Used for display
      purposes.
    - **name**: The user’s name. Used for display purposes.
    - **profile**: User’s profile, like Administrator,
      Editor, UserAdmin etc...
    - **address**: The user’s address.
    - **state**: The user’s state.
    - **zip**: The user’s address zip code.
    - **country**: The user’s country.
    - **email**: The user’s email address.
    - **organisation**: The user’s organisation.
    - **kind**:

Example response for a user::
  
      <users>
          <user>
              <id>3</id>
              <username>eddi</username>
              <surname>Smith</surname>
              <name>John</name>
              <profile>Editor</profile>
              <address/>
              <state/>
              <zip/>
              <country/>
              <email/>
              <organisation/>
              <kind>gov</kind>
          </user>
      </users>

Status
^^^^^^

- **statusvalues**: This is the container for the metadata status value information.
 
  - **status \[0..n]**: A metadata status value. This element has an id attribute
    which represents the local identifier of the status value.

    - **name**: The status value name
    - **reserved**: If 'y' then the status value is one of the core status values 
      which should not be removed. Use 'n' for any status values you add.
    - **label**: The localised labels used to show the
      status value in the user interface.

Example response for status::

  <statusvalues>
    <status id="0">
      <name>unknown</name>
      <reserved>y</reserved>
      <label>
        <eng>Unknown</eng>
      </label>
    </status>
    ...
  </statusvalues>


Schemas
^^^^^^^

- **schemas**: This is the container for the schema information

  - **schema \[0..n]**: A metadata schema.

    - **name** - the name of the schema - this is the name by which the schema is known to GeoNetwork. It is also the name of the directory in ``GEONETWORK_DATA_DIR/config/schema_plugins`` under which the schema can be found.
    - **id** - A unique identifier assigned to the schema in the ``schema-ident.xml`` file.
    - **version** - a version string assigned to the schema in the ``schema-ident.xml`` file.
    - **namespaces** - namespaces used by the metadata schema and records that belong to that schema. This is a string suitable for use as a namespace definition in an XML file.
    - **edit** - if true then records that use this schema can be edited by GeoNetwork, if false then they can't.
    - **conversions** - information about the GeoNetwork services that can be called to convert metadata that use this schema into other XML formats. If there are valid conversions registered for this schema then this element will have a **converter** child for each one of these conversions. Each **converter** child has the following attributes which are intended to be used when searching for a particular format that may be produced by a conversion:

      - **name** - the name of the GeoNetwork service that invokes the converter
      - **nsUri** - the namespace URI of the XML produced by the conversion
      - **schemaLocation** - the schema location (URL) of the namespace URI
      - **xslt** - the name of the XSLT in the plugin schema convert subdirectory that is invoked by the GeoNetwork service to carry out the conversion.

Example response for schemas:

::
 
 <schemas>
  <schema>
    <name>iso19139</name>
    <id>3f95190a-dde4-11df-8626-001c2346de4c</id>
    <version>1.0</version>
    <namespaces>xmlns:gts="http://www.isotc211.org/2005/gts" xmlns:gmx="http://www.isotc211.org/2005/gmx" xmlns:gco="http://www.isotc211.org/2005/gco" xmlns:srv="http://www.isotc211.org/2005/srv" xmlns:gss="http://www.isotc211.org/2005/gss" xmlns:gml="http://www.opengis.net/gml" xmlns:gsr="http://www.isotc211.org/2005/gsr" xmlns:gmd="http://www.isotc211.org/2005/gmd" xmlns:xlink="http://www.w3.org/1999/xlink"</namespaces>
    <convertDirectory>/usr/local/src/git/geonetwork-2.8.x/web/src/main/webapp/WEB-INF/data/config/schema_plugins/iso19139/convert/</convertDirectory>
    <edit>true</edit>
    <conversions>
      <converter name="xml_iso19139" nsUri="http://www.isotc211.org/2005/gmd" schemaLocation="www.isotc211.org/2005/gmd/gmd.xsd" xslt="" />
      <converter name="xml_iso19139Tooai_dc" nsUri="http://www.openarchives.org/OAI/2.0/" schemaLocation="http://www.openarchives.org/OAI/2.0/oai_dc.xsd" xslt="oai_dc.xsl" />
    </conversions>
  </schema> 
  ...
 </schemas>

Looking at the example schema (iso19139) above, there are two converters. The first is invoked by calling the GeoNetwork service ``xml_iso19139`` (eg. ``http://somehost/geonetwork/srv/eng/xml_iso19139?uuid=<uuid of metadata>``). It produces an XML format with namespace URI ``http://www.isotc211.org/gmd`` with schemaLocation ``http://www.isotc211.org/2005/gmd/gmd.xsd`` and xslt name ``xml_iso19139`` because the xslt attribute is set to the empty string.

Localised entities
``````````````````

Localised entities have a general label element which contains the
localised strings in all supported languages. This element has as many
children as the supported languages. Each child has a name that reflect the
language code while its content is the localised text. Here is an example of
such elements::

    <label>
        <en>Editors</en>
        <fr>Éditeurs</fr>
        <es>Editores</es>
    </label>

xml.forward
-----------

This is just a router service. It is used by JavaScript code to connect to a
remote host because a JavaScript program cannot access a machine other than its
server. For example, it is used by the harvesting web interface to query a
remote host and retrieve the list of site ids.

Request
```````

The service’s request::

    <request>
        <site>
            <url>...</url>
            <type>...</type>
            <account>
                <username>...</username>
                <password>...</password>
            </account>
        </site>
        <params>...</params>
    </request>

Where:

#.  **site**: A container for site information where the request will be forwarded.

#.  **url**: Refers to the remote URL to connect to. Usually it points to a
    GeoNetwork XML service but it can point to any XML service.

#.  **type**: Its only purpose is to distinguish GeoNetwork nodes which use a different
    authentication scheme. The value GeoNetwork refers to these nodes. Any other
    value, or if the element is missing, refers to a generic node.

#.  **account**: This element is optional. If present, the provided credentials will be used to
    authenticate to the remote site.

#.  **params**: This is just a container for the request that must be executed remotely.

Request for info from a remote server::

    <request>
        <site>
            <url>http://mynode.org:8080/geonetwork/srv/en/xml.info</url>
        </site>
        <params>
            <request>
                <type>site<type>
            </request>
        </params>
    </request>

Please note that this service uses the GeoNetwork’s proxy
configuration.

Response
````````

The response is just the response from the remote service.

