# Xtrabackup How to 

## how to backup

```bash
#!/bin/sh

dt=`date +%Y-%m-%d_%H-%M-%S`
bkdir=/u02/backup

## "Improved FLUSH TABLES WITH READ LOCK handling" -- https://www.percona.com/doc/percona-xtrabackup/2.2/innobackupex/improved_ftwrl.html

## show global variables like 'have_backup_locks';
## if YES, there's no need to add those options:
## --lock-wait-threshold=40 --lock-wait-query-type=all
## --lock-wait-timeout=180 --kill-long-queries-timeout=20 
## --kill-long-query-type=all

/usr/bin/innobackupex --tmpdir=/tmp --lock-wait-threshold=40 --lock-wait-query-type=all --lock-wait-timeout=180 --kill-long-queries-timeout=20 --kill-long-query-type=all --socket=/tmp/mysql.sock --compress --compress-threads=4 --galera-info --stream=xbstream $bkdir 1>$bkdir/$dt.xbstream 2>$bkdir/$dt.log

## Delete old backups
find $bkdir/ -type f -name '*.log' -mtime +7 -exec rm {} \;
find $bkdir/ -type f -name '*.xbstream' -mtime +7 -exec rm {} \;
```

## backup to remote host

```bash
# pack
innobackupex --socket=/tmp/mysql.sock --compress --compress-threads=8 --stream=xbstream --parallel=4 /u02/backup/ 2> /u02/backup/backup.log | ssh root@desthost "cat - > /u01/backup/backup.xbstream"

# unpack
innobackupex --socket=/tmp/mysql.sock --compress --compress-threads=8 --stream=xbstream --parallel=4 /u02/backup/ 2> /u02/backup/backup.log | ssh root@desthost "xbstream -x -C /u02/backup"
```

## how to restore
```bash
xbstream -x < backup.xbstream -C /root/backup/  # unpack
innobackupex --decompress /root/backup          # decompress, need qpress
innobackupex --apply-log /root/backup/          # apply log
innobackupex --copy-back /root/backup/          # or --move-back
```

## what is qpress

`qpress` is a portable file archiver using QuickLZ and designed to
utilize fast storage systems to their max. It's often faster than file
copy because the destination is smaller than the source.

## Refers
- https://www.percona.com/doc/percona-xtrabackup/2.4/index.html
- https://www.percona.com/doc/percona-xtrabackup/2.4/innobackupex/improved_ftwrl.html
- https://www.percona.com/doc/percona-server/5.6/management/backup_locks.html
- http://www.quicklz.com/
