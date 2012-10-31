Configuration files
===================

There are configuration files attached which can be copied to

::

    $ /usr/local/etc/
    
In those files are initial configurations of server (bacula-dir.conf and bacula-sd.conf) and client (bacula-fd.conf and bconsole.conf).   

Director configuration (/usr/local/etc/bacula-dir.conf)
=======================================================

Director is largest configuration file of Bacula. It defines Clients, Jobs, FileSets, Storage, Devices, Schedules, Pools and Messages.

Director
--------

Defines director itself. No need to change this section.

::

  Director {          # define myself
  Name = vm-bsd-test-dir        # name of director (used in bacula-fd.conf and bconsole.conf)
  DIR Address = 192.168.154.128 # IP address of server
  DIRport = 9101                # where we listen for UA connections
  QueryFile = "/usr/local/share/bacula/query.sql"    #location of file where bacula puts DB queries
  WorkingDirectory = "/var/db/bacula"
  PidDirectory = "/var/run"
  Maximum Concurrent Jobs = 1   # max number of jobs performed at the same time
  Password = "dupadupa"         # Console password
  Messages = Daemon
  }


Job
---

Defines Bacula tasks like backup, restore or verification. It uses Client, Pool, Schedule, Messages and FileSet resources. You can add as many Jobs as you like.

.. note::
    
    If Job don't have Schedule directive defined you will only be able to run it manually.


JobDefs
-------

This is optional but useful. JobDefs defines defaults for Jobs. That means you don't need to define entirely very similar Jobs.

FileSet
-------

Here you put list of files to backup. You may create as many FileSets as you want.

Schedule
--------

Schedule lets you automate tasks.

Pool
----

Pool is used to group volumes which can be assigned to Jobs. For example you may put incremental backups in one pool and full backups in another.

Storage
-------

Defines devices which will be used be Storage Deamon.

Client
------

Client resource contains information about clients. It is used to authorize each client to contact the Director.

Messages
--------

This resource makes reports about events. It can put output to console, log files or e-mail. 

Catalog
-------

Catalog contains informations about connection to database. This section do not need to be changed.


Storage Daemon (/usr/local/etc/bacula-sd.conf)
==============================================

Storage Daemon holds information about storage devices. SD can be on a different machine then Director.

Storage
-------

Here are global parameters used by Storage Daemon.

Director
--------

It lists Directors which can connect to Storage Daemon.

Device
------

Contains accurate informations about storage devices and how Bacula will use it.  

Messages
--------

Information how Director will be reported by Storage Daemon.


File Daemon/Client (/usr/local/etc/bacula-fd.conf)
==================================================

Client/File Daemon is used to communicate with Bacula server. The resources are Client, Director and messages.

FileDaemon
----------

Defines name, IP address, port, directories and maximum concurrent jobs of Client computer or server.

Director
--------

To Director (bacula-dir) can communicate with Client it needs authorization which is defined here.

Messages
--------

Contains information on what reports will be send to Director.

Bacula Console (/usr/local/etc/bconsole.conf)
=============================================

Console is very easy to configure.

::

  Director { 
  Name =  #name of Director (configured in bacula-dir.conf) 
  Address = #IP address of Client
  DIR Port = 9101 
  Password = "" #Director password (configured in bacula-dir.conf)
  }