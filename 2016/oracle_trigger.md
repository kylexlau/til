# Trigger to monitor a table

```sql
-- create table
create table tab_mon as 
(select sid,username,program,machine,'000.000.000.000' ipadd,sysdate moditime from v$session where 0=1); 

-- create trigger
create or replace trigger tab_mon_mod_tr
  before delete on DSS.synchsvrlogs
  --for each row
begin
  insert into dss.tab_mon
    select sid,
           username,
           program,
           machine,
           sys_context('userenv', 'ip_address'),
           sysdate
      from sys.v_$session
     where audsid = userenv('sessionid');
end;

-- check results
select * from tab_mon;
```
