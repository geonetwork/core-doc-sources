.. _metadata_xml_ownership:

Metadata Ownership services
===========================

These services allow retrieval and management of metadata ownership where the 'owner' of a metadata record is the user who created it. 
Only users with **Administrator** and **UserAdmin**
profiles can execute these services.

.. _metadata.batch.newowner:

Batch new owner (metadata.batch.newowner)
-----------------------------------------

The **metadata.batch.newowner** service
allows you to set the owner of a selected set of metadata records. 

.. note:: This service requires a previous call to the ``metadata.select`` service (see :ref:`metadata.select`) to select metadata records.

.. note:: Only those metadata records for which the user running the service has ownership rights on will be updated. If metadata versioning is on then ownership changes will be recorded in the version history.

Requires authentication: Yes

Request to metadata.batch.newowner
``````````````````````````````````

Once the metadata records have been selected the 
**metadata.batch.newowner** service can be invoked with the following
parameters:

- **user**: (mandatory) New owner user identifier
- **group**: (mandatory) New owner group user identifier

Transfer ownership request example::

  Url:
  http://localhost:8080/geonetwork/srv/en/metadata.batch.newowner

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <user>2</user>
    <group>2</group>
  </request>

Response from metadata.batch.newowner
`````````````````````````````````````

If the request is executed successfully then HTTP 200 status code is
returned. If the request fails an HTTP status code error is returned and
the response contains the XML document with the exception.

Transfer ownership (xml.ownership.transfer)
-------------------------------------------

The **xml.ownership.transfer** service can be
used to transfer ownership and privileges of metadata from one user to another.
This service should be used
with data retrieved from previous invocations to the services :ref:`xml.ownership.editors <xml.ownership.editors>` and :ref:`xml.ownership.groups <xml.ownership.groups>` as described below.

Requires authentication: Yes

Request
```````
Parameters:

- **sourceUser**: (mandatory) Identifier of the user whose metadata will 
  be transferred to a new owner

- **sourceGroup**: (mandatory) Identifier
  of one of the user groups of sourceUser

- **targetUser**: (mandatory) Identifier of the user who will become the new 
  owner of the metadata currently owned by sourceUser 

- **targetGroup**: (mandatory) Identifier
  of one of the user groups of the targetUser

Example: In the next example we are going to transfer the
ownership and privileges of metadata owned of user John (id=2) in
group RWS (id=5) to user Samantha(id=7) in group NLR (id=6)

Transfer ownership request example::

  Url:
  http://localhost:8080/geonetwork/srv/en/xml.ownership.transfer

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <sourceUser>2</sourceUser>
    <sourceGroup>5</sourceGroup>
    <targetUser>7</targetUser>
    <targetGroup>6</targetGroup>
  </request>

Response
````````
The response contains the following fields:

- **response**: This is the container for
  the response
  
  - **privileges**: Number of privileges transferred from source group to target group
  - **metadata**: Number of metadata records transferred from source user to target user

Transfer ownership response example::

  <?xml version="1.0" encoding="UTF-8"?>
  <response>
    <privileges>4</privileges>
    <metadata>2</metadata>
  </response>

Errors
``````

- **Service not allowed (error id: service-not-allowed)**, when the user is not authenticated or his profile has no rights to execute the service. Returned 401 HTTP code

- **Missing parameter (error id: missing-parameter)**, when mandatory parameters are not provided

- **bad-parameter XXXX**, when a mandatory parameter is empty

.. _xml.ownership.editors:

Retrieve metadata owners (xml.ownership.editors)
------------------------------------------------

The **xml.ownership.editors** service can be used to retrieve the users with editor profile that own metadata records.

Requires authentication: Yes

Request
```````

Parameters:

- **None**

Retrieve metadata owners request example::

  Url:
  http://localhost:8080/geonetwork/srv/en/xml.ownership.editors

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request />

Response
````````

This is the structure of the response:

- **root**: This is the container for the response

  - **editor**: Container for each editor user information
  
    - **id**: User identifier
    - **username**: User login
    - **name**: User name
    - **surname**: User surname
    - **profile**: User profile

Retrieve metadata editors response example::

  <?xml version="1.0" encoding="UTF-8"?>
  <root>
    <editor>
      <id>1</id>
      <username>admin</username>
      <name>admin</name>
      <surname>admin</surname>
      <profile>Administrator</profile>
    </editor>
    <editor>
      <id>2</id>
      <username>samantha</username>
      <name>Samantha</name>
      <surname>Smith</surname>
      <profile>Editor</profile>
    </editor>
  </root>

Errors
``````

- **Service not allowed (error id: service-not-allowed)**, when the user is not authenticated or his profile has no rights to execute the service. Returned 401 HTTP code

.. _xml.ownership.groups:

Retrieve groups & users that can be used in metadata ownership transfer (xml.ownership.groups)
----------------------------------------------------------------------------------------------

The **xml.ownership.groups** service retrieves:

- all groups that have been assigned privileges over the metadata records owned by the specified user - these will be the source groups from which ownership can be transferred
- all groups to which the user running the service belongs to. A list of the users assigned to the group who have the editor profile is provided with each group. These are the target groups and editors to which ownership can be transferred. 

Typically the :ref:`xml.ownership.editors` service is used to extract the user ids of editors that are used as parameters to retrieve more detailed information about source groups and target groups & editors.

Request
```````

Parameters:

- **id**: (mandatory) User identifier of the user from whom metadata records will be transferred
- The user id of the user running this service will be used to obtain a list of target groups and editors to which the metadata records belonging to user **id** can be transferred.

Retrieve ownership groups request example::

  Url:
  http://localhost:8080/geonetwork/srv/en/xml.ownership.groups

  Mime-type:
  application/xml

  Post request:
  <?xml version="1.0" encoding="UTF-8"?>
  <request>
    <id>2</id>
  </request>

Response
````````

The structure of the response is as follows:

- **response**: This is the container for the response

 - **group**: A group which has privileges over the metadata records owned by the user with user id **id** (can be multiple **group** elements). These groups can be used as the source group list for the transfer ownership service.

  - **id, name, description, email, referrer, label**: Group information

 - **targetGroup**: A user group to which the user running this service has been assigned (can be multiple **targetGroup** elements). The groups can be used as the target group list and the editors from the groups can be target editors for the transfer ownership service.

  - **id, name, description, email, referrer, label**: Group information
  - **editor**: Users from the group that can edit metadata (can be multiple **editor** elements)

   - **id,surname, name**: Metadata user owner information

Retrieve ownership groups response example::

  <?xml version="1.0" encoding="UTF-8"?>
  <response>
    <group>
      <id>3</id>
      <name>bigmetadatausers</name>
      <description>Big Metadata User Groups</description>
      <email>bigmetadatagroup@mail.net</email>
      <referrer />
      <label>
        <en>Big Metadata Users</en>
      </label>
    </group>
    <targetGroup>
      <id>2</id>
      <name>sample</name>
      <description>Demo group</description>
      <email>group@mail.net</email>
      <referrer />
      <label>
        <en>Sample group</en>
      </label>
      <editor>
        <id>12</id>
        <surname />
        <name />
      </editor>
      <editor>
        <id>13</id>
        <surname />
        <name>Samantha</name>
      </editor>
    </targetGroup>
    <targetGroup>
      <id>6</id>
      <name>RWS</name>
      <description />
      <email />
      <referrer />
      <label>
        <en>RWS</en>
      </label>
      <editor>
        <id>7</id>
        <surname />
        <name>Samantha</name>
      </editor>
    </targetGroup>
    ...
  </response>

Errors
``````

- **Service not allowed (error id:
  service-not-allowed)**, when the user is not
  authenticated or his profile has no rights to execute the
  service. Returned 401 HTTP code

