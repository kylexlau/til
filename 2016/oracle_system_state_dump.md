# How to read System State Dump File

Some search keywords:

- waiting for
- sessions blocked by this session
- Dumping final blocker
- (session) sid: 1888
- Handle=0x543fc49e8 RequestMode=X

A active dataguard standby hanged and crashed, generated a system
state dump before crash. Let's read this real world example:

```bash
# grep 'sessions blocked by this session' /u01/app/oracle/diag/rdbms/msopdg/MSOPDG2/trace/MSOPDG2_ora_51595.trc
    There are 177 sessions blocked by this session.
    There are 7 sessions blocked by this session.
    There are 0 sessions blocked by this session.
    There are 178 sessions blocked by this session.
    There are 12 sessions blocked by this session.
    There are 8 sessions blocked by this session.
    There are 102 sessions blocked by this session.

# less /u01/app/oracle/diag/rdbms/msopdg/MSOPDG2/trace/MSOPDG2_ora_51595.trc
Use '/There are 177 sessions blocked by this session.' to search for this process:

Current Wait Stack:
     0: waiting for 'library cache lock'
        handle address=0x543fc49e8, lock address=0x55bf15878, 100*mode+namespace=0x1e004a0003
        wait_id=15437839 seq_num=37155 snap_id=1
        wait times: snap=14 min 33 sec, exc=14 min 33 sec, total=14 min 33 sec
        wait times: max=15 min 0 sec, heur=14 min 33 sec
        wait counts: calls=296 os=296
        in_wait=1 iflags=0x15a2
    There is at least one session blocking this session.
      Dumping 2 direct blocker(s):
        inst: 2, sid: 1888, ser: 783
        inst: 2, sid: 6288, ser: 593
      Dumping final blocker:
        inst: 2, sid: 1888, ser: 783
    There are 177 sessions blocked by this session.

Use '?PROCESS' to find the process name: PROCESS 26: LGWR

Use '/0x543fc49e8' to find what lgwr was doing with this handle:

      SO: 0x55bf15878, type: 78, owner: 0x53a197610, flag: INIT/-/-/0x00 if: 0x3 c: 0x3
       proc=0x53d115f08, name=LIBRARY OBJECT LOCK, file=kgl.h LINE:8751, pg=0

      LibraryObjectLock:  Address=0x55bf15878 Handle=0x543fc49e8 RequestMode=X CanBeBrokenCount=1 Incarnation=1 ExecutionCount=0         
        
        User=0x535b4ab80 Session=0x535b4ab80 ReferenceCount=0 Flags=[0000] SavepointNum=2 
      LibraryHandle:  Address=0x543fc49e8 Hash=568053f9 LockMode=S PinMode=0 LoadLockMode=0 Status=0 
        ObjectName:  Name=SYS.MSOP   
          FullHashValue=cfc4eb5bdfdd63da9e034098568053f9 Namespace=DBINSTANCE(74) Type=CURSOR(00) Identifier=30 OwnerIdn=0 
        Statistics:  InvalidationCount=0 ExecutionCount=0 LoadCount=0 ActiveLocks=2 TotalLockCount=437895 TotalPinCount=0 
        Counters:  BrokenCount=1 RevocablePointer=1 KeepDependency=0 Version=0 BucketInUse=12793 HandleInUse=12793 HandleReferenceCount=0 
        Concurrency:  DependencyMutex=0x543fc4a98(0, 0, 0, 0) Mutex=0x543fc4b28(0, 982435, 387, 0) 
        Flags=RON/PIN/KEP/BSO/[00810003] 
        WaitersLists:  
          Lock=0x543fc4a78[0x55bf158e8,0x4267e83c8] 
          Pin=0x543fc4a58[0x543fc4a58,0x543fc4a58] 
          LoadLock=0x543fc4ad0[0x543fc4ad0,0x543fc4ad0] 
        Timestamp:  
        HandleReference:  Address=0x543fc4b98 Handle=(nil) Flags=[00]    

Use '/\(session\) sid: 1888' to search for sid 1888.

Then search handle '0x543fc49e8':

SO: 0x5584e0348, type: 78, owner: 0x4deaba8d8, flag: INIT/-/-/0x00 if: 0x3 c: 0x3
       proc=0x531129090, name=LIBRARY OBJECT LOCK, file=kgl.h LINE:8751, pg=0

      LibraryObjectLock:  Address=0x5584e0348 Handle=0x543fc49e8 Mode=S CanBeBrokenCount=1 Incarnation=1 ExecutionCount=0         
        
        User=0x53d777220 Session=0x53d777220 ReferenceCount=1 Flags=CNB/[0001] SavepointNum=18632 
      LibraryHandle:  Address=0x543fc49e8 Hash=568053f9 LockMode=S PinMode=0 LoadLockMode=0 Status=0 
        ObjectName:  Name=SYS.MSOP   
          FullHashValue=cfc4eb5bdfdd63da9e034098568053f9 Namespace=DBINSTANCE(74) Type=CURSOR(00) Identifier=30 OwnerIdn=0 
        Statistics:  InvalidationCount=0 ExecutionCount=0 LoadCount=0 ActiveLocks=2 TotalLockCount=437895 TotalPinCount=0 
        Counters:  BrokenCount=1 RevocablePointer=1 KeepDependency=0 Version=0 BucketInUse=12926 HandleInUse=12926 HandleReferenceCount=0 
        Concurrency:  DependencyMutex=0x543fc4a98(0, 0, 0, 0) Mutex=0x543fc4b28(0, 983369, 387, 0) 
        Flags=RON/PIN/KEP/BSO/[00810003] 
        WaitersLists:  
          Lock=0x543fc4a78[0x55bf158e8,0x4267e83c8] 
          Pin=0x543fc4a58[0x543fc4a58,0x543fc4a58] 
          LoadLock=0x543fc4ad0[0x543fc4ad0,0x543fc4ad0] 
        Timestamp:  
        HandleReference:  Address=0x543fc4b98 Handle=(nil) Flags=[00]   
```

Now we have some clues:

- LGWR process request a X mode lock on Handle 0x543fc49e8
- sid 1888 has a S mode lock on Handle 0x543fc49e8
- sid 1888 blocks LGWR
- LGWR blocks 177 other session who request S mode lock on Handle 0x543fc49e8
- then database Hang and crashed 

And Handle 0x543fc49e8 is a DBINSTANCE type object, I konw nothing about this kind of handle type.
