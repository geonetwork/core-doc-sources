.. _metadata_xml_categories:

Metadata Category services
==========================

Update Categories of a metadata record (metadata.category)
----------------------------------------------------------

The **metadata.category** service updates the
categories of a metadata record using the list of categories provided.

.. note:: The previously assigned categories will be removed. If versioning for the metadata record is on, then the previously assigned categories will be available in the version history.

Requires authentication: Yes

Request to metadata.category service
````````````````````````````````````

Parameters:

- **id** or **uuid**: Identifier of metadata to update

- **_C**: (can be multiple elements)

  - **C**: Category identifier (integer). A list of categories and identifiers is stored in the categories table. It can be retrieved using the :ref:`xml.info` service.

Request example:

**POST**::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.category

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <id>6</id>
    <_1/>
    <_2/>
  </request>

**GET**::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.category?id=6&_1&_2

Response from metadata.category service
```````````````````````````````````````

The response contains the identifier of the metadata whose categories have been updated.

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

.. _metadata.batch.update.categories:

Batch update categories (metadata.batch.update.categories)
----------------------------------------------------------

The **metadata.batch.update.categories** service updates the categories of a selected set of metadata using the categories sent as parameters.

.. note:: This service requires a previous call to the ``metadata.select`` service (see :ref:`metadata.select`) to select the metadata records to update.

.. note:: Only those metadata records for which the user running the service has ownership rights on will be updated and all categories previously assigned will be deleted. If metadata versioning is on then category changes will be recorded in the version history.

Requires authentication: Yes

Request to metadata.batch.update.categories
-------------------------------------------

Parameters:

- **_C**: (can be multiple elements)

  - **C**: Category identifier (integer). A list of categories and identifiers is stored in the categories table. It can be retrieved using the :ref:`xml.info` service.


Example request:

**POST**::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.batch.update.categories

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <_1/>
    <_2/>
  </request>

**GET**::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.batch.update.categories?_1&_2

Response from metadata.batch.update.categories
``````````````````````````````````````````````

If the request executed successfully a HTTP 200 status code is returned.
If the request fails an HTTP status code error is returned and
the response is an XML document with the exception.

Errors
``````

- **Service not allowed (error id:
  service-not-allowed)**, when the user is not
  authenticated or their profile has no rights to execute the
  service. Returns 401 HTTP code
