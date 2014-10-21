========
ownCloud
========

`ownCloud <http://owncloud.org/>`_ provides universal access to your files via the web,
your computer or your mobile devices wherever you are. It also provides a platform to easily
view and synchronize your contacts, calendars and bookmarks across all your devices and enables
basic editing right on the web.

**Key features:**

* preconfigure :index:`ownCloud` with mysql and default access credential
* preconfigure httpd 
* integration with |product| system users and groups
* documentation
* backup ownCloud data with nethserver-backup tool


Installation
============

The installation can be done through the |product| web interface.
After the installation:

* open the url https://your_nethserver_ip/owncloud
* use **admin/Nethesis,1234** as default credentials
* change the default password

LDAP access authentication is enabled by default, so each user can login with its system credentials. 
After the installation a new application widget is added to the |product| web interface dashboard.


Update from ownCloud 5
======================

To update: ::

 yum update nethserver-owncloud

.. note:: The update does not change the current configuration.


Force SSL for client connection
===============================

Connect with *Admin user -> Admin Panel -> Check "Enforce HTTPS"*


LDAP Configuration
==================

.. note:: New installations do not need the LDAP configuration because it is done automatically.

#. Copy the LDAP password using the following command: ::

    cat /var/lib/nethserver/secrets/owncloud

#. Login to ownCloud as administrator
#. Search LDAP user and group backend: *Applications -> LDAP user and group backend*
#. Enable "LDAP user and group backend"
#. Configure server parameters: *Admin -> Admin -> Server tab*
#. Fill "Server" tab with these parameters: ::

    Host: localhost:389
    Port: 389
    DN user: cn=owncloud,dc=directory,dc=nh
    Password: "you can use copied password"
    DN base: dc=directory,dc=nh

#. Fill "User filter" tab with: ::

    Modify coarse filter: (&(objectClass=person)(givenName=*))

#. Fill "Access filter" tab with: ::

    Modify coarse filter: uid=%uid

#. Fill "Group filter" tab with: ::

    Modify coarse filter: (&(objectClass=posixGroup)(memberUid=*))

#. Configure "Advanced" tab with: ::

    Directory settings
        Display username: cn
        User structure base: dc=directory,dc=nh
        Display group name: cn
        Group structure base: dc=directory,dc=nh
        Group-member association -> memberUid

    Special Attributes
        Email field: email

#. Configure "Expert" tab with: ::

    Internal username Attribute: uid
    Click on "Clear Username-LDAP user mapping" 

#. Click the "Save" button