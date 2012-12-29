.. _metadata_xml_versioning:

Metadata Versioning services
============================

Start versioning a metadata record (metadata.version)
-----------------------------------------------------

The **metadata.version** service creates an initial version of the metadata record
and its properties (categories, status, privileges) in the subversion repository.

Requires authentication: Yes

Request to metadata.version service
```````````````````````````````````

Parameters:

- **id** or **uuid**: Identifier of metadata to version

Request example:

**POST**::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.version

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <id>6</id>
  </request>

**GET**::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.version?id=6

Response from metadata.version service
``````````````````````````````````````

The response contains the identifier of the metadata for which versioning has been enabled.

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

- **Operation Not Allowed**, if the user does not have editing rights over the metadata record.

.. _metadata.batch.version:

Batch start versioning (metadata.batch.version)
-----------------------------------------------

For each metadata record in the selected set, **metadata.batch.version** creates an initial version of the metadata record and its properties (categories, status, privileges) in the subversion repository.

.. note:: This service requires a previous call to the ``metadata.select`` service (see :ref:`metadata.select`) to select metadata records.

.. note:: Only those metadata records that the user running the service has editing rights over will be versioned. If a metadata record is already versioned then no action is taken.

Requires authentication: Yes

Request to metadata.batch.version
---------------------------------

Parameters:

**None**

Example request:

**POST**::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.batch.version

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request/>

**GET**::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.batch.version

Response from metadata.batch.version
````````````````````````````````````

If the request executed successfully a HTTP 200 status code is
returned. If the request fails an HTTP status code error is returned and
the response contains an XML document with the exception.

Errors
``````

- **Service not allowed (error id:
  service-not-allowed)**, when the user is not
  authenticated or their profile has no rights to execute the
  service. Returns 401 HTTP code
