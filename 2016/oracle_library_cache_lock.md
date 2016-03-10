# Don't perform DDL operations during busy time

You can't perform DDL operations during Oracle database's busy time.
When there are long-running querys on objects you need to perform
DDL. You DDL statements will be blocked.  And all select on that
object after the DDL will be blocked until the long-runing querys are
done.

Grant statements are DDL operations.  Don't execute GRANT on hot
tables at a Oracle database's busy time.

## Reference

From ‘library cache lock’ Waits- Causes and Solutions (Doc ID 1952395.1):

>Do not perform DDL operations during busy periods.
>
>DDL will often cause library cache objects to be invalidated and this
>could cascade to many different dependent objects like
>cursors. Invalidations have a large impact on the library cache,
>shared pool, row cache, and CPU since they will likely require many
>hard parses to occur at the same time.

>Simply schedule DDL during maintenance or low activity periods.

From MOS note Troubleshooting Library Cache: Lock, Pin and Load Lock
(Doc ID 444560.1):

>Be very careful with altering, granting or revoking privileges on
>database objects that frequently used stored PL/SQL is dependent
>on. In fact, resolving this issue mostly depends on application
>project and system maintenance practices. Application developers
>should also consider that some project decisions have negative impact
>to the application scalability and performance.
