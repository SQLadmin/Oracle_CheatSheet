# Install Oracle in Linux

I have installed Oracle 12C release 1 on CentOS and followed the same step for RedHat and Amazon Linux. You can use X windows as well, but I recommend to user Silent installation which saves more time. 

Read my previous page to download Oracle on Linux.

>For this installtion I used Centos 7.2, 

>Server Name ora, 

>IP address 10.10.0.1


## Put a FQDN name for the server
```sh
vi /etc/hosts

127.0.0.1 ora.sqladmin.com ora
10.10.0.1 ora.sqladmin.com ora
```

## Set selinux value to permissive 

```sh
vi /etc/sysconfig/selinux

SELINUX=permissive 
```

## Kernel level parameters
```sh
vi /etc/sysctl.conf

kernel.shmmax = 4294967295
kernel.shmall = 2097152
fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
```

## Update your Server
```sh
sudo yum install epel-release
sudo yum clean metadata && sudo yum upgrade
```

## Reboot to apply all the the config changes
`reboot`


## Install pre-requirement packages
```sh

sudo yum -y install binutils.x86_64 compat-libcap1.x86_64 compat-libstdc++-33.x86_64 compat-libstdc++-33.i686 compat-gcc-44 compat-gcc-44-c++ gcc.x86_64 gcc-c++.x86_64 glibc.i686 glibc.x86_64 glibc-devel.i686 glibc-devel.x86_64 ksh.x86_64 libgcc.i686 libgcc.x86_64 libstdc++.i686 libstdc++.x86_64 libstdc++-devel.i686 libstdc++-devel.x86_64 libaio.i686 libaio.x86_64 libaio-devel.i686 libaio-devel.x86_64 libXext.i686 libXext.x86_64 libXtst.i686 libXtst.x86_64 libX11.x86_64 libX11.i686 libXau.x86_64 libXau.i686 libxcb.i686 libxcb.x86_64 libXi.i686 libXi.x86_64 make.x86_64 unixODBC unixODBC-devel sysstat.x86_64

yum -y install binutils-2.* compat-libstdc++-33* elfutils-libelf-0.* elfutils-libelf-devel-* gcc-4.* gcc-c++-4.* glibc-2.* glibc-common-2.* glibc-devel-2.* glibc-headers-2.* ksh-2* libaio-0.* libaio-devel-0.* libgcc-4.* libstdc++-4.* libstdc++-devel-4.* make-3.* sysstat-7.* unixODBC-2.* unixODBC-devel-2.* 

```

## Add user and groups for oracle

```sh
sudo groupadd -g 54321 oracle
sudo groupadd -g 54322 dba
sudo groupadd -g 54323 oper
sudo useradd -u 54321 -g oracle -G dba,oper oracle
sudo usermod -a -G wheel oracle
sudo passwd oracle
```

## Disable iptables or configure to allow oracle
```sh
sudo iptables -F
sudo service iptables save
sudo chkconfig iptables on
```


## Create swap file
```sh
sudo dd if=/dev/zero of=/swapfile bs=10M count=70
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

### Make swap file at startup
```sh

vi /etc/fstab

/swapfile none swap sw 0 0
```

## Create directories for oracle installation
```sh
sudo mkdir -p /ora01/app/oracle/product/12.1.0/db_1
mkdir -p /ora01/app/oracle/distribs
sudo chown -R oracle:oracle /ora01
sudo chmod -R 775 /ora01
ls -l /ora01
```

## Create bash profile file
```sh
vi /home/oracle/.bash_profile

export TMP=/tmp
export ORACLE_HOSTNAME=ora
export ORACLE_UNQNAME=ORA12C
export ORACLE_BASE=/ora01/app/oracle
export ORACLE_HOME=$ORACLE_BASE/product/12.1.0/db_1
export ORACLE_SID=ORA12C
export PATH=$ORACLE_HOME/bin:$PATH
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:$LD_LIBRARY_PATH
export CLASSPATH=ORACLE_HOME/jlib:ORACLE_HOME/rdbms/jlib;
alias cdob='cd ORACLE_BASE'
alias cdoh='cd ORACLE_HOME'
alias tns='cd ORACLE_HOME/network/admin'
alias envo='env | grep ORACLE'
umask 022
envo
```

## Set file limits and file descriptor values
```sh
vi /etc/security/limits.conf

oracle	soft	nofile	1024	
oracle	hard	nofile	65536	
oracle	soft	nproc	2047
oracle	hard	nproc	16384
oracle	soft	stack	10240
oracle	hard	stack	32768
```

```sh

vi /etc/security/limits.d/20-nproc.conf

-- By default it was set to
* soft nproc 1024
-- We need to change it to.
* - nproc 16384
```

## Installation:

We are doing this installation in three parts.

* `db_install.rsp` – used to install oracle binaries, install/upgrade a database in silent mode
* `dbca.rsp` – used to install/configure/delete a database in silent mode
* `netca.rsp` – used to configure simple network for oracle database in silent mode

### db_install file installation

Move oracle installer to appropriate directory. I have downloaded and extracted Oracle software in /home/ubuntu/database.

```sh

mv /home/oracle/database /ora01/app/oracle/distribs
sudo chown -R oracle:oracle /ora01/app/oracle/distribs/database
sudo chmod -R 775 /ora01/app/oracle/distribs/database

```

>### Now Login as Oracle user.


**Edit and install db_install.rsp**
```sh

cp db_install.rsp db_install.rsp.bck

vi /ora01/app/oracle/distribs/database/response/db_install.rsp

oracle.install.responseFileVersion=/oracle/install/rspfmt_dbinstall_response_schema_v12.1.0
oracle.install.option=INSTALL_DB_SWONLY
ORACLE_HOSTNAME=ora
UNIX_GROUP_NAME=oracle
INVENTORY_LOCATION=/ora01/app/oraInventory
SELECTED_LANGUAGES=en
ORACLE_HOME=/ora01/app/oracle/product/12.1.0/db_1
ORACLE_BASE=/ora01/app/oracle
oracle.install.db.InstallEdition=EE
oracle.install.db.DBA_GROUP=oracle
oracle.install.db.OPER_GROUP=oracle
oracle.install.db.BACKUPDBA_GROUP=oracle
oracle.install.db.DGDBA_GROUP=oracle
oracle.install.db.KMDBA_GROUP=oracle
DECLINE_SECURITY_UPDATES=true

```
Save and close.

Call the source file

`source ~/.bash_profile`

**Install**
```sh
cd /ora01/app/oracle/distribs/database/

./runInstaller -silent -responseFile /ora01/app/oracle/distribs/database/response/db_install.rsp
```

Once its done it’ll show to execute two sh files in root user.

```sh

su root
/ora01/app/oraInventory/orainstRoot.sh
/ora01/app/oracle/product/12.1.0/db_1/root.sh

```

Now test the installation.
```sh

source ~/.bash_profile
sqlplus / as sysdba

```
### netca.rsp file installation
You can edit netca.rsp to set own parameters. I didn’t changed anything here. So just start standard configuration. It will configure LISTENER with standard settings.

```sh

netca -silent -responseFile /ora01/app/oracle/distribs/database/response/netca.rsp
```
Check LISTENER status

`lsnrctl status`

### dbca.rsp file installation 
Lets install the database software, you can also use this to install container database. But here I’m going to install single instance database called ORA12C.

_Make the directories for data files_
```sh
su oracle
mkdir /ora01/app/oracle/oradata
mkdir /ora01/app/oracle/flash_recovery_area
```
Edit the dbca.rsp file

```sh
vi /ora01/app/oracle/distribs/database/response/dbca.rsp

GDBNAME = "ora_master"
SID = "ORA12C"
TEMPLATENAME = "General_Purpose.dbc"
SYSPASSWORD = "oracle"
SYSTEMPASSWORD = "oracle"
EMCONFIGURATION = "DBEXPRESS"
EMEXPRESSPORT = "5500"
SYSMANPASSWORD = "oracle"
DBSNMPPASSWORD = "oracle"
DATAFILEDESTINATION = /ora01/app/oracle/oradata
RECOVERYAREADESTINATION = /ora01/app/oracle/flash_recovery_area
STORAGETYPE = FS
LISTENERS = "LISTENER"
DATABASETYPE = "OLTP"
AUTOMATICMEMORYMANAGEMENT = "TRUE"
TOTALMEMORY = "1024"
```
> `TOTALMEMORY` = Please set this value as 70% of your total memory.

Now execute the below command to create the database.
```sh
dbca -silent -responseFile /ora01/app/oracle/distribs/database/response/dbca.rsp
```

Thats it!! :) 


