# Find the high watermark for datafiles 

```sql
-- find the high watermark
select a.file_name, a.bytes / 1024 / 1024 M, b.max_mb
  from dba_data_files a,
       (select file_id,
               max((block_id + blocks - 1) * 8192) / 1024 / 1024 max_mb
          from dba_extents
         group by file_id) b
 where a.file_id = b.file_id;

-- resize datafiles
select 'alter database datafile ''' || a.file_name || ''' resize ' ||
       round(b.max_mb + 1, 0) || 'M;'
  from dba_data_files a,
       (select file_id,
               max((block_id + blocks - 1) * 8192) / 1024 / 1024 max_mb
          from dba_extents
         group by file_id) b
 where a.file_id = b.file_id;
 ```
