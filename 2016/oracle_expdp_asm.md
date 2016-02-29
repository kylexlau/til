# Expdp to ASM diskgroup

Expdp Log file can't save to ASM diskgroup.

```sql
ASMCMD> mkdir expdpbackup 

create or replace directory exp_reco as '+RECOC1/expdpbackup';

expdp system/****** directory=expdp_dir dumpfile=$DMP_FILE logfile=EXPDP_DIR:$LOG_FILE full=y 
```



