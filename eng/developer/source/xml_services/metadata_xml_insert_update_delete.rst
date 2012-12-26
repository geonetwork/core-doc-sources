.. _metadata_xml_insert_update_delete:

Metadata insert, update and delete services
===========================================

These services provide insert, update and delete operations for metadata records. They could be used by a metadata editing program external to GeoNetwork.

This is the Create, Update, Delete part of the metadata CRUD operations in GeoNetwork. For read/retrieve operations (the R in CRUD) see :ref:`metadata_xml_search_retrieve`.

Insert metadata (metadata.insert)
---------------------------------

The **metadata.insert** service allows you to insert a new record into the catalogue. 

Requires authentication: Yes

Request
```````

Parameters:

- **data**: (mandatory) Contains the
  metadata record

- **group** (mandatory): Owner group
  identifier for metadata

- **isTemplate**: indicates if the
  metadata content is a new template or not. Default value:
  "n"

- **title**: Metadata title. Only
  required if isTemplate = "y"

- **category** (mandatory): Metadata
  category. Use "_none_" value to don't assign any
  category

- **styleSheet** (mandatory): Stylesheet
  name to transform the metadata before inserting in the
  catalog. Use "_none_" value to don't apply any
  stylesheet

- **validate**: Indicates if the metadata
  should be validated before inserting in the catalog. Values:
  on, off (default)

Insert metadata request example::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.insert

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <group>2</group>
    <category>_none_</category>
    <styleSheet>_none_</styleSheet>
    <data><![CDATA[
      <gmd:MD_Metadata xmlns:gmd="http://www.isotc211.org/2005/gmd"
                   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      ...
         </gmd:DQ_DataQuality>
        </gmd:dataQualityInfo>
      </gmd:MD_Metadata>]]>
    </data>
  </request>

Response
````````

If request is executed succesfully HTTP 200 status code is
returned. If request fails an HTTP status code error is returned and
the response contains the XML document with the exception.

If validate parameter is set to "on" and the provided metadata
is not valid with respect to the XSD and schematrons in use for the metadata 
schema then an exception report is returned.

Example validation metadata report::

  <?xml version="1.0" encoding="UTF-8"?>
  <error id="xsd-validation-error">
    <message>XSD Validation error(s)</message>
    <class>XSDValidationErrorEx</class>
    <stack>
      <at class="org.fao.geonet.services.metadata.ImportFromDir"
        file="ImportFromDir.java" line="297" method="validateIt" />
      <at class="org.fao.geonet.services.metadata.ImportFromDir"
        file="ImportFromDir.java" line="281" method="validateIt" />
      <at class="org.fao.geonet.services.metadata.Insert"
        file="Insert.java" line="102" method="exec" />
      <at class="jeeves.server.dispatchers.ServiceInfo"
        file="ServiceInfo.java" line="238" method="execService" />
      <at class="jeeves.server.dispatchers.ServiceInfo"
        file="ServiceInfo.java" line="141" method="execServices" />
      <at class="jeeves.server.dispatchers.ServiceManager"
        file="ServiceManager.java" line="377" method="dispatch" />
      <at class="jeeves.server.JeevesEngine"
        file="JeevesEngine.java" line="621" method="dispatch" />
      <at class="jeeves.server.sources.http.JeevesServlet"
        file="JeevesServlet.java" line="174" method="execute" />
      <at class="jeeves.server.sources.http.JeevesServlet"
        file="JeevesServlet.java" line="99" method="doPost" />
      <at class="javax.servlet.http.HttpServlet"
        file="HttpServlet.java" line="727" method="service" />
    </stack>
    <object>
      <xsderrors>
        <error>
          <message>ERROR(1) org.xml.sax.SAXParseException: cvc-datatype-valid.1.2.1: '' is not a valid value for 'dateTime'. (Element: gco:DateTime with parent element: gmd:date)</message>
          <xpath>gmd:identificationInfo/gmd:MD_DataIdentification/gmd:citation/gmd:CI_Citation/gmd:date/gmd:CI_Date/gmd:date/gco:DateTime</xpath>
        </error>
        <error>
          <message>ERROR(2) org.xml.sax.SAXParseException: cvc-type.3.1.3: The value '' of element 'gco:DateTime' is not valid. (Element: gco:DateTime with parent element: gmd:date)</message>
          <xpath>gmd:identificationInfo/gmd:MD_DataIdentification/gmd:citation/gmd:CI_Citation/gmd:date/gmd:CI_Date/gmd:date/gco:DateTime</xpath>
        </error>
        <error>
          <message>ERROR(3) org.xml.sax.SAXParseException: cvc-datatype-valid.1.2.1: '' is not a valid value for 'integer'. (Element: gco:Integer with parent element: gmd:denominator)</message>
          <xpath>gmd:identificationInfo/gmd:MD_DataIdentification/gmd:spatialResolution/gmd:MD_Resolution/gmd:equivalentScale/gmd:MD_RepresentativeFraction/gmd:denominator/gco:Integer</xpath>
        </error>
        <error>
          <message>ERROR(4) org.xml.sax.SAXParseException: cvc-type.3.1.3: The value '' of element 'gco:Integer' is not valid. (Element: gco:Integer with parent element: gmd:denominator)</message>
          <xpath>gmd:identificationInfo/gmd:MD_DataIdentification/gmd:spatialResolution/gmd:MD_Resolution/gmd:equivalentScale/gmd:MD_RepresentativeFraction/gmd:denominator/gco:Integer</xpath>
        </error>
      </xsderrors>
    </object>
    <request>
      <language>en</language>
      <service>metadata.insert</service>
    </request>
  </error>

Errors
``````

- **Service not allowed (error id:
  service-not-allowed)**, when the user is not
  authenticated or their profile has no rights to execute the
  service. Returned 401 HTTP code

- **Missing parameter (error id:
  missing-parameter)**, when mandatory parameters are
  not provided. Returns 400 HTTP code

- **bad-parameter XXXX**, when a
  mandatory parameter is empty. Returns 400 HTTP code

- **ERROR: duplicate key violates unique
  constraint "metadata_uuid_key"**, if another
  metadata record in catalog has the same uuid of the metadata
  record being inserted

Update metadata (metadata.update)
---------------------------------

The metadata.update service allows you to update a metadata record in the catalog.

Requires authentication: Yes

Request
```````

Parameters:

- **id**: (mandatory) Identifier of the metadata to update

- **version**: (mandatory) This parameter
  is used to check if another user has updated the metadata
  after we retrieved it and before invoking the update metadata
  service. **CHECK how to provide value to
  the user**

- **isTemplate**: indicates if the
  metadata content is a new template or not. Default value: "n"

- **showValidationErrors**: Indicates if
  the metadata should be validated before updating in the
  catalog.

- **title**: Metadata title (for templates)

- **data** (mandatory) Contains the metadata record

Update metadata request example::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.update

  Mime-type:
  application/xml

  Post request:

  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <id>11</id>
    **<version>1</version>**
    <data><![CDATA[
      <gmd:MD_Metadata xmlns:gmd="http://www.isotc211.org/2005/gmd"
                       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      
      ...
      
            </gmd:DQ_DataQuality>
        </gmd:dataQualityInfo>
      </gmd:MD_Metadata>]]>
    </data>
  </request>

Response
````````

If request is executed succesfully HTTP 200 status code is
returned. If request fails an HTTP status code error is returned and
the response contains the XML document with the exception.

Errors
``````

- **Service not allowed (error id:
  service-not-allowed)**, when the user is not
  authenticated or his profile has no rights to execute the
  service. Returned 401 HTTP code

- **Missing parameter (error id:
  missing-parameter)**, when mandatory parameters are
  not provided. Returned 400 HTTP code

- **bad-parameter XXXX**, when a
  mandatory parameter is empty. Returned 400 HTTP code

- **Concurrent update (error id:
  client)**, when the version number provided is
  different from actual version number for metatada. Returned
  400 HTTP code

Delete metadata (metadata.delete)
---------------------------------

The **metadata.delete** service allows to
remove a metadata record from the catalog. The metadata content is
backup in MEF format by default in data\\removed folder. This folder
can be configured in geonetwork\\WEB-INF\\config.xml.

Requires authentication: Yes

Request
```````

Parameters:

- **id**: (mandatory) Identifier of the metadata to delete

Delete metadata request example::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.delete

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <id>10</id>
  </request>

Response
````````

If request is executed succesfully HTTP 200 status code is
returned. If request fails an HTTP status code error is returned and
the response contains the XML document with the exception.

Errors
``````

- **Service not allowed (error id:
  service-not-allowed)**, when the user is not
  authenticated or his profile has no rights to execute the
  service. Returned 401 HTTP code

- **Metadata not found (error id:
  error)**, if the identifier provided don't correspond
  to an existing metadata. Returned 500 HTTP code

- **Operation not allowed** **(error id: operation-not-allowed)**, when
  the user is not authorized to edit the metadata. To edit a metadata:
  
  - The user is the metadata owner
  - The user is an Administrator
  - The user has edit rights over the metadata
  - The user is a Reviewer and/or UserAdmin and the
    metadata groupOwner is one of his groups


