# SHOW ENGINE

## Syntax

```sql
SHOW ENGINE engine_name {STATUS | MUTEX}
```

## Description

<code class="highlight fixed" style="white-space:pre-wrap">SHOW ENGINE</code> displays operational information about a storage
engine.  The following statements currently are supported:

```sql
SHOW ENGINE INNODB STATUS
SHOW ENGINE INNODB MUTEX
SHOW ENGINE PERFORMANCE_SCHEMA STATUS
SHOW ENGINE ROCKSDB STATUS
```

If the [Sphinx Storage Engine](/kb/en/sphinxse/) is installed, the following is also supported:

```sql
SHOW ENGINE SPHINX STATUS
```

See <a undefined>SHOW ENGINE SPHINX STATUS</a>.

Older (and now removed) synonyms were <code class="highlight fixed" style="white-space:pre-wrap">SHOW INNODB STATUS</code>
for <code class="highlight fixed" style="white-space:pre-wrap">SHOW ENGINE INNODB STATUS</code> and 
<code class="highlight fixed" style="white-space:pre-wrap">SHOW MUTEX STATUS</code> for 
<code class="highlight fixed" style="white-space:pre-wrap">SHOW ENGINE INNODB MUTEX</code>.

### SHOW ENGINE INNODB STATUS

<code class="highlight fixed" style="white-space:pre-wrap">SHOW ENGINE INNODB STATUS</code> displays extensive information
from the standard InnoDB Monitor about the state of the InnoDB storage engine.
See [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) for more.

### SHOW ENGINE INNODB MUTEX

<code class="highlight fixed" style="white-space:pre-wrap">SHOW ENGINE INNODB MUTEX</code> displays InnoDB mutex statistics.

The statement displays the following output fields:

- <strong>Type:</strong> Always InnoDB.
- <strong>Name:</strong> The source file where the mutex is implemented, and the line number
  in the file where the mutex is created. The line number is dependent on the MariaDB version.
- <strong>Status:</strong> This field displays the following values if `UNIV_DEBUG` was defined at compilation time (for example, in include/univ.h in the InnoDB part of the source tree). Only the `os_waits` value is displayed if `UNIV_DEBUG` was not defined. Without `UNIV_DEBUG`, the information on which the output is based is insufficient to distinguish regular mutexes and mutexes that protect
  rw-locks (which allow multiple readers or a single writer). Consequently, the
  output may appear to contain multiple rows for the same mutex.
<ul start="1"><li><strong>count</strong> indicates how many times the mutex was requested.
</li><li><strong>spin_waits</strong> indicates how many times the spinlock had to run.
</li><li><strong>spin_rounds</strong> indicates the number of spinlock rounds. (spin_rounds divided by
   spin_waits provides the average round count.)
</li><li><strong>os_waits</strong> indicates the number of operating system waits. This occurs when
   the spinlock did not work (the mutex was not locked during the spinlock and
   it was necessary to yield to the operating system and wait).
</li><li><strong>os_yields</strong> indicates the number of times a the thread trying to lock a mutex
   gave up its timeslice and yielded to the operating system (on the
   presumption that allowing other threads to run will free the mutex so that
   it can be locked).
</li><li><strong>os_wait_times</strong> indicates the amount of time (in ms) spent in operating system
   waits, if the timed_mutexes system variable is 1 (ON). If timed_mutexes is 0
   (OFF), timing is disabled, so os_wait_times is 0. timed_mutexes is off by
   default.
</li></ul>

Information from this statement can be used to diagnose system problems. For
example, large values of spin_waits and spin_rounds may indicate scalability
problems.

The [information_schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema).<a undefined>INNODB_MUTEXES</a> table provides similar information.

### SHOW ENGINE PERFORMANCE_SCHEMA STATUS

This statement shows how much memory is used for [performance_schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema) tables and internal buffers.

The output contains the following fields:

- <strong>Type:</strong> Always `performance_schema`.
- <strong>Name:</strong> The name of a table, the name of an internal buffer, or the `performance_schema` word, followed by a dot and an attribute. Internal buffers names are enclosed by parenthesis. `performance_schema` means that the attribute refers to the whole database (it is a total).
- <strong>Status:</strong> The value for the attribute.

The following attributes are shown, in this order, for all tables:

- <strong>row_size:</strong> The memory used for an individual record. This value will never change.
- <strong>row_count:</strong> The number of rows in the table or buffer. For some tables, this value depends on a server system variable.
- <strong>memory:</strong> For tables and `performance_schema`, this is the result of `row_size` * `row_count`.

For internal buffers, the attributes are:

- <strong>count</strong>
- <strong>size</strong>

### SHOW ENGINE ROCKSDB STATUS

See also [MyRocks Performance Troubleshooting](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-performance-troubleshooting)