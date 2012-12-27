.. _metadata_xml_status:

Metadata Status services
========================

Update Status on a metadata record (metadata.status)
----------------------------------------------------

The **metadata.status** service updates the
status on a metadata record using the status and changeMessage provided
as parameters. 

.. note:: The previously assigned status will be removed. If versioning for the metadata record is on, then the previously assigned status will be available in the version history.

Requires authentication: Yes

Request to metadata.status service
``````````````````````````````````

Parameters:

- **id** or **uuid**: Identifier of metadata to update

- **status**: One of the status identifiers take from the database table ``statusvalues``. Status identifiers (see :ref:`xml.metadata.status.values.list`) are:

 - 0: unknown
 - 1: draft
 - 2: approved
 - 3: retired
 - 4: submitted
 - 5: rejected

- **changeMessage**: description of why the status has changed.

Request example:

**POST**::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.status

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <id>6</id>
    <status>5</status>
    <changeMessage>Completely unacceptable: consistency rules ignored<changeMessage/>
  </request>

**GET**::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.status?id=6&status=5&changeMessage=Do%20it%20all%20again%20nitwit

.. note:: URL encoding of changeMessage.

Response from metadata.status service
`````````````````````````````````````

The response contains the identifier of the metadata whose status has been updated.

Example::

  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <id>6</id>
  </request>

Errors
``````

- **Service not allowed (error id:
  service-not-allowed)**, when the user is not
  authenticated or their profile has no rights to execute the
  service. Returns 401 HTTP code

- **Metadata not found (error id: metadata-not-found)** if 
  a metadata record with the identifier provided does not exist

- **Only the owner of the metadata can set the status. User is not the owner of the metadata**, if the user does not have ownership rights over the metadata record.

.. _metadata.batch.update.status:

Batch update status (metadata.batch.update.status)
--------------------------------------------------

The **metadata.batch.update.status** service updates the status on a selected set of metadata using the status and changeMessage sent as parameters.

.. note:: Only those metadata records for which the user running the service has ownership rights on will be updated and all status values previously assigned will be deleted. If metadata versioning is on then status changes will be recorded in the version history.

This service requires a previous call to **metadata.select** service to select the metadata records to update.

Requires authentication: Yes

.. include:: metadata_xml_select_include.rst

Request to metadata.batch.update.status
---------------------------------------

Parameters:

- **status**: One of the status identifiers take from the database table ``statusvalues``. Status identifiers (see :ref:`xml.metadata.status.values.list`):

 - 0: unknown
 - 1: draft
 - 2: approved
 - 3: retired
 - 4: submitted
 - 5: rejected

- **changeMessage**: description of why the status has changed.

Example request:

**POST**::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.batch.update.status

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <status>5</status>
    <changeMessage>Completely unacceptable: consistency rules ignored<changeMessage/>
  </request>

**GET**::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.batch.update.status?&status=5&changeMessage=Do%20it%20all%20again%20nitwit

.. note:: URL encoding of changeMessage.

Response from metadata.batch.update.status
``````````````````````````````````````````

If the request executed successfully a HTTP 200 status code is
returned. If the request fails an HTTP status code error is returned and
the response contains the XML document with the exception.

Errors
``````

- **Service not allowed (error id:
  service-not-allowed)**, when the user is not
  authenticated or their profile has no rights to execute the
  service. Returns 401 HTTP code

.. _xml.metadata.status.values.list:

Get status values (xml.metadata.status.values.list)
---------------------------------------------------

This service gets the status values and identifiers defined in the status table in the GeoNetwork database. It is intended to be used when building a request to update the status of a metadata record.

.. note:: This service will very likely move into xml.info in the next version of GeoNetwork. See :ref:`xml.info`.

Requires authentication: No

Request
```````

Parameters: **None**

Example:

**POST**::

  Url:
  http://localhost:8080/geonetwork/srv/en/xml.metadata.status.values.list

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request/>

**GET**::

  Url:
  http://localhost:8080/geonetwork/srv/en/xml.metadata.status.values.list

Response
````````

If the request executed successfully a HTTP 200 status code is
returned and the XML with status values and identifiers. An example follows::

 <response>
   <record>
     <id>0</id>
     <name>unknown</name>
     <reserved>y</reserved>
     <label>
       <eng>Unknown</eng>
     </label>
   </record>
   <record>
     <id>1</id>
     <name>draft</name>
     <reserved>y</reserved>
     <label>
       <eng>Draft</eng>
     </label>
   </record>
   ...
 </response>

Get status of a metadata record (xml.metadata.status.get)
---------------------------------------------------------

This service gets the status of a particular metadata record specified by id or uuid as a parameter. 

Requires authentication: No.

Request
```````

Parameters:

- **id** or **uuid**: Identifier of metadata to obtain status of.

Response
````````

If the request executed successfully a HTTP 200 status code is
returned and the XML with status values for the metadata record (note: all changesin status are present) is returned. An example follows::

 <response>
   <record>
    <statusid>5</statusid>
    <userid>4</userid>
    <changedate>2012-12-27T14:58:04</changedate>
    <changemessage>Do it all again</changemessage>
    <name>rejected</name>
   </record>
   <record>
    <statusid>4</statusid>
    <userid>6</userid>
    <changedate>2012-12-27T14:32:10</changedate>
    <changemessage>Ready for review</changemessage>
    <name>submitted</name>
   </record>
  </response> 

Defining status actions
-----------------------

The behaviour of GeoNetwork when a status changes can be defined by the programmer.  See :ref:`java_metadata_status_actions`.
