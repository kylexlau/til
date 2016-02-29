# Script to purge all entries in dba_2pc_pending view

```sql
select count(*) from dba_2pc_pending;

-- purge 
declare 
  tid varchar2(50);
  stmt varchar2(200);
begin 
  for tid in (select local_tran_id from dba_2pc_pending)
    loop 
      stmt := 'dbms_transaction.purge_lost_db_entry(''' || tid.local_tran_id || ''')';
      execute immediate 'call ' || stmt;
      commit;
    end loop;
    --commit;
end;
/

-- disable auto distributed recovery
alter system disable distributed recovery;
```
