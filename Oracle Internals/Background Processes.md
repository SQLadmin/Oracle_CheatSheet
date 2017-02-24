# Oracle Background Process

Whenever the Oracle is stated then there will be some processes also started which are mandatory to the keep Oracle up and running. I'm going to talk about the most common and important process. 


* Database Writer Process
* Log Writer Process
* Checkpoint Process
* System Monitor Process
* Process Monitor Process
* Archiver Process
* Recovery Process

![](https://github.com/SqlAdmin/Oracle_CheatSheet/blob/master/Images/Oracle%20background%20Processes.JPG)

### Database Writer process(DBWR):
This process is responsible for write the data from Data buffer cache to Data files.

### Log Writer Process(LGWR):
This process is responsible for write redo logs from Redo buffer cache to Redo log files.
**Some intersting points**
1. Once the redo buffer write to the disk then the next redo buffer will for previous commits acknowledement.
1. All other waiting redo buffers will write to the disk as a group commit. So the I/O will reduce.
1. `DBWR` will write the data files after the Redo logs written to the Disk.


### Checkpoint Process:
Checkpoints will occur when the transaction got committed, Once the transaction is committed then this process will update the header of the data files to record the details of the checkpoint.

### System Monitor Process(SMON):
This process will start when the Oracle is started. It is responsible for recovery kind of things. During startup of the database, it will go and check if any uncommitted transactions and rollback them. 

### Process Monitor Process(PMON):
Whenever the user's session got killed or abnormally terminated then this process is responsible for clean up that user's session data, temp data and etc.

### Archiver Process(ARCn):
There is no use for keeping the redo logs for so long time, so this process will help to archive the Redo logs to archive log files.

### Recovery Process(RECO):
This process is intended for recovery of distributed databases. The distributed transaction recovery process finds pending distributed transactions and resolves them.  All in-doubt transactions are recovered by this process in the distributed database setup. RECO will connect to the remote database to resolve pending transactions.

### Reference Links:
[Reference Video](https://www.youtube.com/watch?v=zCKWRKSGlA0&t=2s)

[A blog post](http://satya-dba.blogspot.in/2009/08/background-processes-in-oracle.html)