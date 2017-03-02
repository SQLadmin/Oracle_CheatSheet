# Oracle Physical Files Structure

Oracle has the below important physical files to help the Oracle instance and Database to start and work. Without anyone of these files Oracle never start.

* Parameter Files
  * Spfile
  * Pfile
  * password File
* Data Files
  * Data file
  * Control file
  * Undo log file.
  * Redo log file
* Diagnostic Files:
  * Alert Log file
  * Trace File
* Other Files:
  * Backups files
  * Archive Log files

![](https://github.com/SqlAdmin/Oracle_CheatSheet/blob/master/Images/Oracle%20physical%20files.png)

## Spfile
Spfile is a binary files that contains parameters to control the oracle instance and Database.If any changes made on this file it'll apply to all running and future instances.

## Pfile:
Pfile is a normal text file, this also contains the parameter to configure the Instance setting and database settings, If any changes made on this file you must restart the instances.

> **Note:** While the oracle startup this parameter files only called first, it has the name and location of the control files.

## Password File:
This file has the user names and passwords. This will use when a user login with SYSDBA privileges. 

## Data Files:
it has the exact data that we insert into the database. 

## Control file: 
The control file is the main part of oracle. it has the information about the all database names, database file names, tablespace names and their location. And also it contains the backup information of a database.

## Undo log files: 
When you change data you should be able to either rollback that change or to provide a read consistent view of the original data.

## Redo log files:
Redo logs used for recovery purpose. It records all the activity. The transaction must be committed to the Redo logs first, then it'll commit to the data files.

## Alert files:
This file is responsible for to store the information about the Startup, Shutdown, Tablespace creation and etc. its recommended to DBA's at least to check this log once a day.

## Trace Files:
Traces files are debugging files which can trace background process information (LGWR, DBWn, etc), core dump information (ora-600 errors, etc) and user processing information (SQL).

## Backup files:
This is nothing just the files of oracle database backup which are taken from RMAN or Export. 

## Archive Logs:
Archive logs are nothing but old redo log file, there is no use for keeping redo logs for a long time, So we can archive this redo logs. 

### Read More:

[A blog post](http://www.datadisk.co.uk/html_docs/oracle/structure.htm)