Administration
==============

When configuration of services is done it is advised to check if everything works as intended.

Starting services:

::
  
    $ /usr/local/etc/rc.d/bacula-dir start
    $ /usr/local/etc/rc.d/bacula-sd start
    
On Client:

::

    $ /usr/local/etc/rc.d/bacula-fd start

Now we need to see if everything is running fine. On server and client:

::

    $ ps auwx | grep bacula
    
If output suggests that Bacula's services are up and running we should check if they are listening on configured ports. Curently configured IP's are 192.168.154.129 for server and 192.168.154.129 for client.

On server:

::
  
    $ netstat -an | grep '192.168.154.128'
  
On client

::

    $ netstat -an | grep '192.168.154.129'
    
When output says that services listen on configured tcp ports we are good to do backups.

Console program
---------------

To start console program on client, type:

::

    $ bconsole
    
Console is configured to contact the Director configured in bconsole.conf. From this point user or administrator is able to manage Bacula.

Usage:

::

    * <command> <keyword1>[=<argument1>]

For example:

::

    * list files jobid=2

This will display all files saved for jobid 2.

Running a Job
-------------

To run a Job you need to type run <return>

:: 

    * run <return>
    Automatically selected Catalog: BaculaCatalog
    Using Catalog "BaculaCatalog"
    A job name must be specified.
    The defined Job resources are:
        1: Test
        2: Backup Catalog
        3: RestoreFiles
        
    Select Job resource (1-3):    
        
As you cam see Bacula already knows, that we have only one Catalog defind so it picks it up automiatically. It also read all defined jobs from *bacula-dir.conf*

Lets say we want to run Job Test (1):

::

    Select Job resource (1-3): 1 <return>
    Run Backup job
    JobName:  Test
    Level:    Full
    Client:   vm-bsd-client-fd
    FileSet:  Full Set
    Pool:     File (From Job resource)
    Storage:  HDD (From Job resource)
    When:     2012-10-31 14:09:44
    Priority: 10
    OK to run? (yes/no/mod):
    
Output is informing user what Bacula is going to do. Job Test was previusly defined in Director (bacula-dir.conf). Console gives a choice to run this job, to not run this job or to modify it.

If you type 'mod' it will show you parameters which you can modify.

  * Level
  * Storage
  * Job
  * FileSet
  * Client
  * When
  * Priority
  * Pool
  * Plugin Options

After finishing you can simply run this and check the results how it went.

::

    * status <return>
    Status available for:
        1: Director
        2: Storage
        3: Client
        4: All
    Select daemon type for status (1-4): 
    
Lets ask Director how our job went:

::

    Select daemon type for status (1-4): 1
    
    Running Jobs:
    Console connected at 31-oct-12 14:02
    No Jobs running.
    ====
    
    Terminated Jobs:
     JobId   Level     Files      Bytes   Status   Finished          Name 
    ========================================================================
         1   Full           3    208.6 K  OK       31-oct-12 14:380  Test    
         
    ====
    
          