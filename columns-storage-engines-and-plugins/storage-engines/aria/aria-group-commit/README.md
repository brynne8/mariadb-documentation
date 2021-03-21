# Aria Group Commit

Since [MariaDB 5.2](/kb/en/what-is-mariadb-52/), the [Aria storage engine](/columns-storage-engines-and-plugins/storage-engines/aria) has included a feature to group commits to speed up concurrent threads doing many inserts into the same or different Aria tables.

By default, group commit for Aria is turned off. It is controlled by the 
[aria_group_commit](/kb/en/aria-server-system-variables/#aria_group_commit) and [aria_group_commit_interval](/kb/en/aria-server-system-variables/#aria_group_commit_interval) system variables.

Information on setting server variables can be found on the [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables) page.

## Terminology

- A `commit` is `flush of logs` followed by a sync.
- `sent to disk` means written to disk but not sync()ed,
- `flushed` mean sent to disk and synced().
- `LSN` means log serial number. It's refers to the position in the transaction log.

## Non Group commit logic (aria_group_commit="none")

The thread which first started the `commit` is performing the actual flush of logs. Other threads set the new goal (LSN)
of the next pass (if it is maximum) and wait for the pass end or just wait for the pass end.

The effect of this is that a flush (write of logs + sync) will save all data for all threads/transactions that have been waiting since the last flush.

## If hard group commit is enabled (aria_group_commit="hard")

### If hard commit and aria_group_commit_interval=0

The first thread sends all changed buffers to disk. This is repeated as long as there are new LSNs added. The process can not loop
forever because we have a limited number of threads and they will wait for the data to be synced.

Pseudo code:

```sql
do
   send changed buffers to disk
 while new_goal
sync
```

### If hard commit and aria_group_commit_interval &gt; 0

If less than rate microseconds has passed since the last sync, then after buffers have been sent to disk, wait until rate microseconds has passed since last sync, do sync and return.
This ensures that if we call sync infrequently we don't do any waits.

## If soft group commit is enabled (aria_group_commit="soft")

Note that soft group commit should only be used if you can afford to lose a few rows if your machine shuts down hard (as in the case of a power failure).

Works like in `non group commit'` but the thread doesn't do any real sync(). If aria_group_commit_interval is not zero, the sync() will be performed by a service thread with the given rate when needed (new LSN appears).  If aria_group_commit_interval is zero, there will be no sync() calls.

## Code

The code for this can be found in storage/maria/ma_loghandler.c::translog_flush()