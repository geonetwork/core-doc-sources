
.. _metadata_xml_select:

Metadata Select Services
========================

These services are for creating and managing a set of selected metadata records. The selected set is normally used by the metadata.batch services eg. :ref:`metadata.batch.update.privileges`, :ref:`metadata.batch.newowner`, :ref:`metadata.batch.update.status`, :ref:`metadata.batch.update.categories`.

.. _metadata.select:

Request to metadata.select service
``````````````````````````````````

Parameters:

- **id**: Identifier of metadata to select (can be more than one)

- **selected**: Selection state. Values: add, add-all, remove, remove-all

Select all metadata example::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.select

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <selected>add-all</selected>
  </request>

Select a metadata record example::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.select

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <id>2</id>
    <selected>add</selected>
  </request>

Clear metadata selection example::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.select

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <selected>remove-all</selected>
  </request>

Response from metadata.select service
`````````````````````````````````````

The response contains the number of metadata records selected.

Example::

  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <Selected>10</Selected>
  </request>

