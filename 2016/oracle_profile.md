# Configure Oracle Profile

## check

```sql
select username,profile from dba_user;

select * from dba_profiles order by PROFILE;
```

## create verify function

```sql
---- Comment out those lines
-- ALTER PROFILE DEFAULT LIMIT
-- PASSWORD_LIFE_TIME 180
-- PASSWORD_GRACE_TIME 7
-- PASSWORD_REUSE_TIME UNLIMITED
-- PASSWORD_REUSE_MAX UNLIMITED
-- FAILED_LOGIN_ATTEMPTS 10
-- PASSWORD_LOCK_TIME 1
-- PASSWORD_VERIFY_FUNCTION verify_function_11G;

---- execute
SQL> @?/rdbms/admin/utlpwdmg.sql
```

## create

```sql
create profile USER_PROFILE limit
  sessions_per_user unlimited
  cpu_per_session unlimited
  cpu_per_call unlimited
  connect_time unlimited
  idle_time 15
  logical_reads_per_session unlimited
  logical_reads_per_call unlimited
  composite_limit unlimited
  private_sga unlimited
  failed_login_attempts 5
  password_life_time 90
  password_reuse_time 60
  password_reuse_max 5
  password_lock_time 15/1440
  password_grace_time unlimited
  password_verify_function verify_function_11g;
```

## apply
```sql
alter user username profile USER_PROFILE;
```

## refer
- [Database SQL Language Reference - CREATE PROFILE](http://docs.oracle.com/cd/E11882_01/server.112/e41084/statements_6010.htm)
