.. _metadata_xml_privileges:

Metadata Privilege services
===========================

Update privileges on a metadata record (metadata.admin)
-------------------------------------------------------

The **metadata.admin** service updates the
privileges on a metadata record using a list of groups and privileges sent 
as parameters. 

.. note:: All previously assigned privileges will be deleted. If versioning for the metadata record is on, then the previously assigned privileges will be available in the version history.

Requires authentication: Yes

Request to metadata.admin service
`````````````````````````````````

Parameters:

- **id**: Identifier of metadata to update

- **_G_O**: (can be multiple elements)

  - **G**: Group identifier
  - **O**: Privilege (Operation) identifier. Privilege identifiers:

   - 0: view
   - 1: download
   - 2: editing
   - 3: notify
   - 4: dynamic
   - 5: featured

Request example:

**POST**::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.admin

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <id>6</id>
    <_1_2 />
    <_1_1 />
  </request>

**GET**::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.admin?id=6&_1_2&_1_1

Response from metadata.admin service
````````````````````````````````````

The response contains the identifier of the metadata whose privileges have been updated.

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

- **ERROR: insert or update on table "operationallowed"
  violates foreign key 'operationallowed_operationid_fkey »**, if an
  operation identifier provided is not valid

- **ERROR: insert or update on table "operationallowed"
  violates foreign key 'operationallowed_groupid_fkey »**, if a
  group identifier provided is not valid

.. _metadata.batch.update.privileges:

Batch update privileges (metadata.batch.update.privileges)
----------------------------------------------------------

The **metadata.batch.update.privileges** service updates the privileges on a selected set of metadata using the list of groups and privileges sent as parameters.

.. note:: Only those metadata records for which the user running the service has ownership rights on will be updated and all privileges previously assigned will be deleted.

This service requires a previous call to **metadata.select** service to select the metadata records to update.

Requires authentication: Yes

.. include:: metadata_xml_select_include.rst

Request to metadata.batch.update.privileges
-------------------------------------------

Parameters:

- **_G_O**: (can be multiple elements)

  - **G**: Group identifier
  - **O**: Privilege (Operation) identifier. Privilege identifiers:

   - 0: view
   - 1: download
   - 2: editing
   - 3: notify
   - 4: dynamic
   - 5: featured

Example request:

**POST**::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.batch.update.privileges

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <_1_2 />
    <_1_1 />
  </request>

**GET**::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.batch.update.privileges?_1_2&_1_1

Response from metadata.batch.update.privileges
``````````````````````````````````````````````

If the request executed successfully a HTTP 200 status code is
returned and some XML summarizing what has changed. An example is shown below:

::

 <response>
   <done>7</done>
   <notOwner>0</notOwner>
   <notFound>0</notFound>
 </response>

Description of elements in response:

- **response** - container for response
 - **done** - number of selected metadata records processed
 - **notOwner** - number of selected records skipped because user running service was not owner
 - **notFound** - number of selected records that were not found when retrieved (eg. may have been deleted)

If the request fails an HTTP status code error is returned and
the response contains the XML document with the exception.

Errors
``````

- **Service not allowed (error id:
  service-not-allowed)**, when the user is not
  authenticated or their profile has no rights to execute the
  service. Returns 401 HTTP code

- **ERROR: insert or update on table "operationallowed"
  violates foreign key 'operationallowed_operationid_fkey »**, if an
  operation identifier provided is not valid

- **ERROR: insert or update on table "operationallowed"
  violates foreign key 'operationallowed_groupid_fkey »**, if a
  group identifier provided is not valid

