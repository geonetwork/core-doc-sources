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

Request to metadata.update service
``````````````````````````````````

Parameters:

- **id**: Identifier of metadata to update

- **status**: One of the status identifiers take from the database table ``statusvalues``. Status identifiers are:

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

- **status**: One of the status identifiers take from the database table ``statusvalues``. Status identifiers:

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
returned and some XML summarizing what has changed. An example is shown below:

::

 <response>
   <done>7</done>
   <notOwner>0</notOwner>
   <notFound>0</notFound>
   <noChange>7</noChange>
 </response>

Description of elements in response:

- **response** - container for response
 - **done** - number of selected metadata records processed
 - **notOwner** - number of selected records skipped because user running service was not owner
 - **notFound** - number of selected records that were not found when retrieved (eg. may have been deleted)
 - **noChange** - number of selected records that were unchanged by the batch operation.

If the request fails an HTTP status code error is returned and
the response contains the XML document with the exception.

Errors
``````

- **Service not allowed (error id:
  service-not-allowed)**, when the user is not
  authenticated or their profile has no rights to execute the
  service. Returns 401 HTTP code

- **Metadata not found (error id: metadata-not-found)** if the 
  metadata record with the identifier provided does not exist

Defining status actions
-----------------------

The behaviour of GeoNetwork when a status changes can be defined by the programmer.  See :ref:`java_metadata_status_actions`.
