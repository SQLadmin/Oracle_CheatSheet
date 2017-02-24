# Oracle Architrecture

A well-crafted architecture will help the Oracle to do its best. Basically, the architecture is the both combination of physical and logical components. Here I have divided the Oracle's Architecture into four blocks.

1. Process block
1. Memory block
1. Storage block
1. Background process block

I'll explain each block in several posts, now I'm just going to explain how Oracle is architected. 

![](https://github.com/SqlAdmin/Oracle_CheatSheet/blob/master/Images/Oracle%20Architrecture.jpg?raw=true|alt=oracle architecture)

Let's  assume that we are going to start with an example query 

````select * from customer;````

## Process Block:
In this block, the user process is nothing just the user's query which has been executed in SqlPlus. Then the SQLplus pass this query to the Oracle, now this user process is converted to System Process which means the Oracle will be handling this process. Whenever a process is started in by oracle a particular amount of Memory will allocate to that process. This memory is called Process Global Area(PGA). These are the things will happen in the Process block.

## Memory Block:
Oracle has two types of memory allocations.
* Program Global Area
* System Global Area

We already saw what is PGA. SGA is nothing but a group of shared memory areas that are dedicated to an Oracle instance. 

**Data Dictionary Cache:**

Now the select query process will go Data dictionary Cache, this cache contains the information about other database objects like tables owners, permission, etc. 

**Data Buffer Cache:**

So if the user has access to that table then it'll start getting the data from the table and store it in Data buffer and give the results to us. 

**Redo Buffer:** 

All the changes which are happening inside the database will right Redo buffers first and flushed to the Redo log files.

## Storage Block:

This block contains the actual physical data files such as Control files, Redo log files, password files, etc.


## Background process Block:

This block contains some background services which are owned by oracle. For example DBWR, this process will write the data from Data buffer cache to Data files and LGWR will write redo logs from redo buffer.


### Reference 
[Reference Video](https://www.youtube.com/watch?v=8_YXIj7sh8M)