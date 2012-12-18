.. _configuration:

.. toctree::
   :maxdepth: 2

Basic configuration
===================

System configuration
--------------------

Many GeoNetwork System configuration parameters can be changed using the
web interface. Database parameters can be changed using the GAST application.

.. important:: Configuration of these parameters is critically important for a proper
   functioning of the GeoNetwork catalogue in an operational context. Failing to
   properly change these settings may result in a system that does not function as
   expected. For example, downloads may fail to be correctly processed, or metadata
   harvesting from other servers may not work.

To get to the System configuration parameters, you must be logged on as administrator first. Open the Administration page and select System configuration (The link is inside the red ellipse).

.. important:: New installations of GeoNetwork use admin for both username
   and password. It is important to change this from the Administration page once
   you logged on!

.. figure:: web-config-where.png

    *The link to the System configuration page*

Clicking the page’s link you will get the set of parameters that you can change. A detailed description of these parameters follows:

.. figure:: web-config-options-1.png

    *The configuration options - part 1*

.. figure:: web-config-options-2.png

    *The configuration options - part 2*

.. figure:: web-config-options-3.png

    *The configuration options - part 3*

Firstly, at the bottom of the page (you will need to scroll down) there are three buttons with the following purpose:

 - **Back** Simply returns to the main administration page, ignoring any changes you may have made. 
 - **Save** Saves the current options. If some options are invalid, the system will show a dialogue with the wrong parameter and will focus its text field on the page. Once the configuration is saved a success dialogue will be shown. 
 - **Refresh** This button refreshes the displa
 yed options taking the new values from the server. This can be useful if some options get changed dynamically (for example by another user).

Site parameters
```````````````

*Catalogue identifier* A universally unique identifier (uuid) that distinguishes your catalogue from any other catalogue. This a unique identifier for your catalogue and its best to leave it as a uuid. 

*Name* The name of the GeoNetwork node. Information that helps identify the catalogue to a human user.

*Organization* The organization the node belongs to. Again, this is information that helps identify the catalogue to a human user.

Server parameters
`````````````````

Here you have to enter the details of the web address of your GeoNetwork node. This address is important because it will be used to build addresses that access services and data on the GeoNetwork node. In particular:

#. building links to data file uploaded with a metadata record in the editor.
#. the CSW server is asked to describe it's capabilities. The GetCapabilities operation returns an XML document with HTTP links to the CSW services provided by the server. These links are dynamically built using the host and port values.


*Protocol* The HTTP protocol used to access the server. Choosing http means that all communication with GeoNetwork will be visible to anyone listening to the protocol. Since this includes usernames and passwords this is not secure. Choosing https means that all communication with GeoNetwork will be encrypted and thus much harder for a listener to decode. 

*Host* The node’s address or IP number. If your node is publicly accessible from the Internet, you have to use the machine’s domain/address. If your node is hidden into your private network and you have a firewall or web server that redirects incoming calls to the node, you have to enter the public address of the firewall or web server. A typical configuration is to have an Apache web server on address A that is publicly accessible and redirects the requests to a Tomcat server on a private address B. In this case you have to enter A in the host parameter.

*Port* The node’s port (usually 80 or 8080). If the node is hidden, you have to enter the port on the public firewall or web server. 


Intranet Parameters
```````````````````

A common need for an organisation is to discriminate between internal anonymous users (users that access the node from within the organisation) and external ones (users from the Internet). Node’s administrators can specify different privileges for internal and external anonymous users and, in order to do so, they have to specify the parameters of the internal network.

*Network* The internal network’s address in IP form.

*Netmask* The network’s mask.


Metadata Search Results
```````````````````````

Configuration settings in this group determine what the limits are on user interaction with the search results.

*Maximum Selected Records* The maximum number of search results that a user can select and process with the batch operations eg. Set Privileges, Categories etc.


Multi-Threaded Indexing
```````````````````````

Configuration settings in this group determine how many processor threads are allocated to indexing tasks in GeoNetwork. If your machine has many processor cores, you can now determine how many to allocate to GeoNetwork indexing tasks. This can bring dramatic speed improvements on large indexing tasks (eg. changing the privileges on 20,000 records) because GeoNetwork can split the indexing task into a number of pieces and assign them to different processor cores.

*Number of processing threads* The maximum number of processing threads that can be allocated to an indexing task. 

Note: this option is only available for databases that have been tested. Those databases are PostGIS and Oracle.

Lucene Index Optimizer
```````````````````````

Configuration settings in this group determine when the Lucene Index Optimizer is run. By default, this takes place at midnight each day. With recent upgrades to Lucene, particularly Lucene 3.6.1, the optimizer is becoming less useful, so this configuration group will very likely be removed in future versions.

Z39.50 configuration
~~~~~~~~~~~~~~~~~~~~

GeoNetwork can act as a Z39.50 server. Z39.50 is an older communication protocol used for distributed searching.

*Enable*: Check this option to start the Z39.50 server. Please, notice that GeoNetwork must be restarted in order to make this change active.

*port*: This is the port on which GeoNetwork will be
listening for incoming Z39.50 requests. Z3950 servers can run on different ports, but 210, 2100 and 6668 are common choices.
To have multiple GeoNetwork nodes on the same machine you
have to change this value in order to avoid port conflicts between the
different nodes.

XLink resolver
``````````````
Enables/disables the XLink resolver. XLink resolver replaces content of the elements with attribute @xlink:href (except for srv:operatesOn element) with the referenced content. The 
XLink resolver is used for example in Metadata fragments harvester to retrieve the metadata fragments referenced in the metadata and insert in it.


Clickable hyperlinks
````````````````````
Enables/disables hyperlinks in metadata content for urls.


Automatic fixes
```````````````
Enabled by default. It is recommended you do not use the GeoNetwork default or advanced editor when auto-changing is disabled.
See http://trac.osgeo.org/geonetwork/ticket/368 for more details.


INSPIRE
```````
Enables/disables the INSPIRE search options in advanced search panel.

Proxy configuration
```````````````````

*Proxy*: In some occasions (like harvesting) GeoNetwork must
be able to connect to remote sites and this may be denied if an organisation
uses proxy servers. In this cases, GeoNetwork must be configured to use the
proxy server in order to route outgoing requests.

.. figure:: web-config-options-proxy.png

    *The proxy configuration options*

*Host*: The proxy’s name or address to use (usually an IP address).

*Port*: The proxy’s port to use.

*Username* (optional): a username should be provided if the proxy server requires authentication.

*Password* (optional): a password should be provided if the proxy server requires authentication.

Email & notification
````````````````````

*Feedback* GeoNetwork can sometimes send email, for example if a metadata is downloaded or
if a user provides feedback using the online form. You have to configure
the mail server GeoNetwork should use in order to enable it to send email.

.. figure:: web-config-options-mailserver.png

    *The mail server configuration options*

*Email*: This is the email address that will be used to send
the email (the From address).

*SMTP host*: the mail server address to use when sending
email.

*SMTP port*: the mail server SMTP port (usually 25).

Removed metadata
````````````````

Defines the directory used to store a backup of metadata and data after a delete action. This
directory is used as a backup directory to allow system administrators to recover metadata and possibly
related data after erroneous deletion. By default the removed directory
is created under the data folder

Authentication
``````````````

In this section you define the source against which GeoNetwork will authenticate users and passwords.

.. figure:: web-config-options-authentication.png

    *Authentication configuration options*

By default, users are authenticated against info held in the GeoNetwork database. When the GeoNetwork database is used as the authentication source, the user self-registration function can be enabled. A later section discusses user self-registration and the configuration options it requires.

You may choose to authenticate logins against either the GeoNetwork database tables or LDAP (the lightweight directory access protocol) but not both. The next section describes how to authenticate against LDAP.

In addition to either of these options, you may also configure other authentication sources. At present, Shibboleth is the only additional authentication source that can be configured. Shibboleth is typically used for national access federations such as the Australian Access Federation. Configuring shibboleth authentication in GeoNetwork to use such a federation would allow not only users from a local database or LDAP directory to use your installation, but any user from such a federation.

LDAP Authentication
~~~~~~~~~~~~~~~~~~~

The section defines how to connect to an LDAP authentication system.

.. figure:: web-config-options-ldap.png

    *The LDAP configuration options*

Typically all users must have their details in the LDAP directory to login to GeoNetwork. However if a user is added to the GeoNetwork database with the Administrator profile then they will be able to login without their details being present in the LDAP directory.

Shibboleth Authentication
~~~~~~~~~~~~~~~~~~~~~~~~~

When using either the GeoNetwork database or LDAP for authentication, you can also configure shibboleth to allow authentication against access federations.

.. figure:: web-config-options-shibboleth.png

    *The Shibboleth configuration options*

Shibboleth authentication requires interaction with Apache web server. In particular, the apache web server must be configured to require Shibboleth authentication to access the path entered in the configuration. The apache web server configuration will contain the details of the shibboleth server that works out where a user is located (sometimes called a 'where are you from' server).

The remainder of the shibboleth login configuration describes how shibboleth authentication attributes are mapped to GeoNetwork user database fields as once a user is authenticated against shibboleth, their details are copied to the local GeoNetwork database.

Configuring OGC Catalogue Services Web (CSW)
````````````````````````````````````````````
See :doc:`../cswserver/index`

Configuring User Self-Registration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

GeoNetwork has a self-registration function which allows a user to request a login which provides access to 'registered-user' functions.  By default this capability is switched off. To configure this capability you must complete the following sections in the 'System configuration' menu:

- configure the site name and organization name as these will be used in emails from this GeoNetwork site to newly registered users. An example of how to config these fields at the top of the system configuration form is:

.. figure:: web-config-name-organization.png

- configure feedback email address, SMTP host and SMTP port. The feedback email address will be sent an email when a new user registers and requests a profile other than 'Registered User'. An example of how to config these fields in the system configuration form is:

.. figure:: web-config-options-feedback.png

- check the box, enable user self-registration in the Authentication section of the system configuration form as follows:

.. figure:: web-config-authentication-self-registration-checked.png

When you save the system configuration form, return to the home page and log out as admin, your banner menu should now include two new options, 'Forgot your password?' and 'Register' (or their translations into your selected language) as follows:

.. figure:: web-config-banner-with-self-registration.png

You should also configure the xml file that includes contact details to be 
displayed when an error occurs in the registration process. This file is 
localized - the english version is located in 
INSTALL_DIR/web/geonetwork/loc/en/xml/registration-sent.xml.

Finally, if you want to change the content of the email that contains registration details for new users, you should modify INSTALL_DIR/web/geonetwork/xsl/registration-pwd-email.xsl.
