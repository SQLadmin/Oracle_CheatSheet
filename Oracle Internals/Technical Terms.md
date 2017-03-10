
# Oracle Technical Terms

Before stating with oracle we should know the basic technical terms.


### **SID**:

The Oracle System ID (SID) is used to `uniquely identify a particular database` on a system.

SID = DataBaseName + Instance Name. 

### **DATABASE:**

Nothing its just a of a database which is the collection of data files, Redo Log files, Control file and etc. In RAC a database can be shared to multiple instanaces.

### **INSTANCE:**

Instanace is a `collection of all the backgroud process` of oracle. While startup the oracle should be start the instance first, so it includes Memory process(SGA and PGA), SMON,PMON, DBwR, etc. Once the instance is started  then it'll start the database. 

### **DATABASE DOMAIN:**

Usually the same as your company domain (somecompany.com).

### **GLOBAL DATABASE NAME:**

GDN = Database name + database domain (somedb.somecompany.com).

### **SERVICE NAME:** 

A database service name is a service created in listener to capture the application connections. Whenever we configure listener, the service name will be given where it listens/captures the connections from client and connects to DB using service
and OS/User/Password authentication

### **Lintener.ora:** 

A file that has to be configured on oracle server to resolve incoming connection requests to the listener on `Oracle Sever`.

### **tnsnames.ora:** 

Afile that has to be configured on the client computer to tell the client computer the address where the listener is located.

### **UNIQUE DB NAME:**

The ORACLE_UNQNAME is an operating system environment variable that holds the database’s unique name value.

### **oraenv:**

Set of environment variables for linux system.

### **Oracle_Home:**

a directory where the Oracle software is installed;

### **Oracle_Base:**

ORACLE_BASE is an environment variable used as base directory for an OFA installation.