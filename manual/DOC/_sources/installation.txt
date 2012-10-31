Installation of Bacula server
=============================

What is needed to install Bacula on FreeBSD system:

  - FreeBSD 8.x or 9.x (optionally with ports collection) installed on physical or virtual machine,
  - Root provliges to make changes in system files,
  - MySQL 5.x (prefered), SQLite or PostGRE SQL database.

There are two ways to install Bacula on FreeBSD:

  - using ports collection,
  - compiling from source.
  
As we want it to be as simple as it can we will install it using ports

::
  
    $ cd /usr/ports/sysutils/bacula-server
    $ make install
  
When Bacula installer asks for prefered database we select MySQL (which is already installed).

After installer finishes his job we need to create database for Bacula, make tables and grant privliges to bacula user.

.. note::
    This step might be slightly different depending on bacula version. I recommend using TAB key to automatically fill version number.
    
.. note::
    If there is root password set for MySQL you need to put *-p* to the following commands and you will be prompted for password.

::
    
      $ cd /usr/ports/sysutils/bacula-server/work/bacula-5.0.3/src/cats
      $ ./grant_mysql_privileges
      $ ./create_mysql_database
      $ ./make_mysql_tables

    
Running Bacula
--------------

.. note::
    Before running Bacula we should configure it first, however we will execute first run at this point to check if everything goes smooth.
    
Editing rc.conf (you may use any text editor you want like vim or ee)

::

    $ ee /etc/rc.conf

At the end of file we put: ::
    
    bacula_dir_enable="YES"
    bacula_sd_enable="YES"
    bacula_fd_enable="YES"

This will make Bacula start at system boot. To manually start services type: ::
    
    $ /usr/local/etc/rc.d/bacula-dir start
    $ /usr/local/etc/rc.d/bacula-sd start
    
Check if the processes are running: ::
    
    $ ps auwx | grep bacula
    
If output is fine we stop the services and go to configuration files of Bacula. ::
    
    $ /usr/local/etc/rc.d/bacula-dir stop
    $ /usr/local/etc/rc.d/bacula-sd stop

Installation of Bacula File Daemon/Client
=========================================

The installation of Bacula Client is very similar to server installation.

::
  
    $ cd /usr/ports/sysutils/bacula-client
    $ make install
    

Installation of Bacula Console
===============================

Bacula Console (bconsole) is part of Bacula Daemon and it installs with it.  