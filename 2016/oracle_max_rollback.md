# The max rollback segments of a Oracle database is 32760

```sql
# sqlplus  / as sysdba

SQL*Plus: Release 11.2.0.2.0 Production on Mon Mar 21 10:30:21 2016

Copyright (c) 1982, 2010, Oracle.  All rights reserved.


Connected to:
Oracle Database 11g Enterprise Edition Release 11.2.0.2.0 - 64bit Production
With the Partitioning, OLAP, Data Mining and Real Application Testing options

SQL> select count(*) from dba_rollback_segs;

  COUNT(*)
----------
        11
SQL> alter system set "_rollback_segment_count"=33000 scope=spfile;

System altered.

SQL> shutdown immediate 
Database closed.
Database dismounted.
ORACLE instance shut down.

SQL> startup
ORACLE instance started.

Total System Global Area 1.0689E+10 bytes
Fixed Size                  2235904 bytes
Variable Size            8220836352 bytes
Database Buffers         2449473536 bytes
Redo Buffers               16928768 bytes
Database mounted.
Database opened.

SQL> select count(*) from dba_rollback_segs;

  COUNT(*)
----------
     32760

SQL> create undo tablespace undots1 datafile '/u01/app/oracle/oradata/test/undots1.dbf' size 1G;

Tablespace created.

SQL> alter system set undo_tablespace=UNDOTS1;

System altered.

SQL> select count(*) from dba_rollback_segs;

  COUNT(*)
----------
     32760

SQL> conn scott/tiger;
Connected.
SQL> create table t (n number);

Table created.

SQL> insert into t values(1);
insert into t values(1)
            *
ERROR at line 1:
ORA-01552: cannot use system rollback segment for non-system tablespace 'USERS'

SQL> drop tablespace undotbs1 including contents and datafiles;

Tablespace dropped.

SQL> select count(*) from dba_rollback_segs;

  COUNT(*)
----------
         2

SQL> alter system reset "_rollback_segment_count" scope=spfile;

System altered.

SQL> shutdown immediate 
Database closed.
Database dismounted.
ORACLE instance shut down.
SQL> startup 
ORACLE instance started.

Total System Global Area 1.0689E+10 bytes
Fixed Size                  2235904 bytes
Variable Size            8220836352 bytes
Database Buffers         2449473536 bytes
Redo Buffers               16928768 bytes
Database mounted.
Database opened.
```
