.. _schema_information:

Schema information
==================

Introduction
------------

Metadata schemas can be plugged into GeoNetwork - see :ref:`schemaPlugins`. Any application that needs to know:

- which schemas are plugged into GeoNetwork 
- information about the schema elements and codelists 

can use the services in this section to obtain that information.

xml.schema.list
---------------

This service returns information about the schemas that are currently plugged into GeoNetwork.

Request
```````

No parameters are required.

Response
````````

The response will have information for each schema in a schema element. A typical response with one schema (the others have been removed for brevity) looks something like the following:

::
 
 <response>
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
 </response>


Descriptions of the children of the schema element and their contents:

- **name** - the name of the schema - this is the name by which the schema is known to GeoNetwork. It is also the name of the directory in ``GEONETWORK_DATA_DIR/config/schema_plugins`` under which the schema can be found.
- **id** - A unique identifier assigned to the schema in the ``schema-ident.xml`` file.
- **version** - a version string
- **namespaces** - namespaces used by the metadata schema and records that belong to that schema. This is a string suitable for use as a namespace definition in an XML file.
- **edit** - if true then records that use this schema can be edited by GeoNetwork, if false then they can't.
- **conversions** - information about the GeoNetwork services that can be called to convert metadata that use this schema into other XML formats. If there are valid conversions registered for this schema then this element will have a **converter** child for each one of these conversions. Each **converter** child has the following attributes which are intended to be used when searching for a particular format that may be produced by a conversion:

  - **name** - the name of the GeoNetwork service that invokes the converter
  - **nsUri** - the namespace URI of the XML produced by the conversion
  - **schemaLocation** - the schema location (URL) of the namespace URI
  - **xslt** - the name of the XSLT in the plugin schema convert subdirectory that is invoked by the GeoNetwork service to carry out the conversion.

Looking at the example schema (iso19139) above, there are two converters. The first is invoked by calling the GeoNetwork service ``xml_iso19139`` (eg. ``http://somehost/geonetwork/srv/eng/xml_iso19139?uuid=<uuid of metadata>``). It produces an XML format with namespace URI ``http://www.isotc211.org/gmd`` with schemaLocation ``http://www.isotc211.org/2005/gmd/gmd.xsd`` and xslt name ``xml_iso19139`` because the xslt attribute is set to the empty string.

xml.schema.info
---------------

This service returns information about a set of schema elements or codelists.
The returned information consists of a localised label, a description,
conditions that the element must satisfy etc...

Request
```````

Due to its nature, this service accepts only the POST binding with
application/XML content type. The request can contain
several element and codelist elements. Each element indicate the will to
retrieve information for that element. Here follows the element
descriptions:

- **element**: It must contain a **schema** and a **name** attribute. The first
  one must be one of the supported schemas (see the section above).
  The second must be the qualified name of the element which
  information must be retrieved. The namespace must be declared into
  this element or into the root element of the request.

- **codelist**: Works like the previous one but returns information
  about codelists.

::

    <request xmlns:gmd="http://www.isotc211.org/2005/gmd">
        <element schema="iso19139" name="gmd:constraintLanguage" />
        <codelist schema="iso19115" name="DateTypCd" />
    </request>

.. note:: The returned text is localised depending on the language specified during
  the service call. A call to /geonetwork/srv/en/xml.schema.info
  will return text in the English language.

Response
````````

The response’s root element will be populated with information of the
elements/codelists specified into the request. The structure is the
following:

- **element**: A container for information about an element. It has a
  name attribute which contains the qualified name of the element.

  - **label**: The human readable name of the element, localised
    into the request’s language.
  - **description**: A generic description of the element.
  - **condition \[0..1]**: This element is optional and indicates
    if the element must satisfy a condition, like the element is
    always mandatory or is mandatory if another one is
    missing.

- **codelist**: A container for information about a codelist. It has a
  name attribute which contains the qualified name of the codelist.

  - **entry \[1..n]**: A container for a codelist entry. There can
    be many entries.

    - **code**: The entry’s code. This is the value that
      will be present inside the metadata.
    - **label**: This is a human readable name, used to
      show the entry into the user interface. It is
      localised.
    - **description**: A generic localised description of
      the codelist.

::

    <response>
        <element name="gmd:constraintLanguage">
            <label>Constraint language</label>
            <description>language used in Application Schema</description>
            <condition>mandatory</condition>
        </element>
        <codelist name="DateTypCd">
            <entry>
                <code>creation</code>
                <label>Creation</label>
                <description>date when the resource was brought into existence</description>
            </entry>
            <entry>
                <code>publication</code>
                <label>Publication</label>
                <description>date when the resource was issued</description>
            </entry>
            <entry>
                <code>revision</code>
                <label>Revision</label>
                <description>date identifies when the resource was examined
                or re-examined and improved or amended</description>
            </entry>
        </codelist>
    </response>

Error management
````````````````

Beside the normal exceptions management, the
service can encounter some errors trying to retrieve an element/codelist
information. In this case, the object is copied verbatim to the response
with the addition of an error attribute that describes the encountered
error. Here follows an example of such response::

    <response>
        <element schema="iso19139" name="blablabla" error="not-found"/>
    </response>

.. _table_schema_errors:

Possible errors returned by xml.schema.info service:

=================   ============================================================
Error code          Description
=================   ============================================================
unknown-schema      The specified schema is not supported
unknown-namespace   The namespace of the specified prefix was not found
not-found           The requested element / codelist was not found
=================   ============================================================


