# The Debug Sync Facility

The Debug Sync Facility allows placement of synchronization points in
the server code by using the DEBUG_SYNC macro:

```sql
open_tables(...)

DEBUG_SYNC(thd, "after_open_tables");

lock_tables(...)
```

When activated, a sync point can

- Emit a signal and/or
- Wait for a signal

<table><tbody><tr><th>Nomenclature</th><th>Description</th></tr>
<tr><td>signal</td><td>A value of a global variable that persists until overwritten by a new signal. The global variable can also be seen as a "signal post" or "flag mast". Then the signal is what is attached to the "signal post" or "flag mast".</td></tr>
<tr><td>emit a signal</td><td>Assign the value (the signal) to the global variable ("set a flag") and broadcast a global condition to wake those waiting for a signal.</td></tr>
<tr><td>wait for a signal</td><td>Loop over waiting for the global condition until the global value matches the wait-for signal.</td></tr>
</tbody></table>

By default, all sync points are inactive. They do nothing (except to
burn a couple of CPU cycles for checking if they are active).

A sync point becomes active when an action is requested for it.
To do so, put a line like this in the test case file:

```sql
SET DEBUG_SYNC= 'after_open_tables SIGNAL opened WAIT_FOR flushed';
```

This activates the sync point
<code class="highlight fixed" style="white-space:pre-wrap">'after_open_tables'</code>. It requests it to emit the
signal <code class="highlight fixed" style="white-space:pre-wrap">'opened'</code> and wait for another thread to emit
the signal <code class="highlight fixed" style="white-space:pre-wrap">'flushed'</code> when the thread's execution
runs through the sync point.

For every sync point there can be one action per thread only. Every
thread can request multiple actions, but only one per sync point. In
other words, a thread can activate multiple sync points.

Here is an example how to activate and use the sync points:

```sql
--connection conn1
SET DEBUG_SYNC= 'after_open_tables SIGNAL opened WAIT_FOR flushed';
send INSERT INTO t1 VALUES(1);
    --connection conn2
    SET DEBUG_SYNC= 'now WAIT_FOR opened';
    SET DEBUG_SYNC= 'after_abort_locks SIGNAL flushed';
    FLUSH TABLE t1;
```

When <code class="highlight fixed" style="white-space:pre-wrap">conn1</code> runs through the
<code class="highlight fixed" style="white-space:pre-wrap">INSERT</code> statement, it hits the sync point
<code class="highlight fixed" style="white-space:pre-wrap">'after_open_tables'</code>. It notices that it is active
and executes its action. It emits the signal
<code class="highlight fixed" style="white-space:pre-wrap">'opened'</code> and waits for another thread to emit the
signal <code class="highlight fixed" style="white-space:pre-wrap">'flushed'</code>.

<code class="highlight fixed" style="white-space:pre-wrap">conn2</code> waits immediately at the special sync point <code class="highlight fixed" style="white-space:pre-wrap">'now'</code> for another thread to emit the <code class="highlight fixed" style="white-space:pre-wrap">'opened'</code> signal.

A signal remains in effect until it is overwritten. If
<code class="highlight fixed" style="white-space:pre-wrap">conn1</code> signals <code class="highlight fixed" style="white-space:pre-wrap">'opened'</code> before
<code class="highlight fixed" style="white-space:pre-wrap">conn2</code> reaches <code class="highlight fixed" style="white-space:pre-wrap">'now'</code>,
<code class="highlight fixed" style="white-space:pre-wrap">conn2</code> will still find the
<code class="highlight fixed" style="white-space:pre-wrap">'opened'</code> signal. It does not wait in this case.

When <code class="highlight fixed" style="white-space:pre-wrap">conn2</code> reaches <code class="highlight fixed" style="white-space:pre-wrap">'after_abort_locks'</code>, it signals <code class="highlight fixed" style="white-space:pre-wrap">'flushed'</code>, which lets <code class="highlight fixed" style="white-space:pre-wrap">conn1</code> awake.

Normally the activation of a sync point is cleared when it has been
executed. Sometimes it is necessary to keep the sync point active for
another execution. You can add an execute count to the action:

```sql
SET DEBUG_SYNC= 'name SIGNAL sig EXECUTE 3';
```

This sets the signal point's activation counter to 3. Each execution
decrements the counter. After the third execution the sync point
becomes inactive.

One of the primary goals of this facility is to eliminate sleeps from
the test suite. In most cases it should be possible to rewrite test
cases so that they do not need to sleep. (But this facility cannot
synchronize multiple processes.) However, to support test development,
and as a last resort, sync point waiting times out. There is a default
timeout, but it can be overridden:

```sql
SET DEBUG_SYNC= 'name WAIT_FOR sig TIMEOUT 10 EXECUTE 2';
```

<code class="highlight fixed" style="white-space:pre-wrap">TIMEOUT 0</code> is special: If the signal is not present,
the wait times out immediately.

When a wait timed out (even on <code class="highlight fixed" style="white-space:pre-wrap">TIMEOUT 0</code>), a
warning is generated so that it shows up in the test result.

You can throw an error message and kill the query when a synchronization
point is hit a certain number of times:

```sql
SET DEBUG_SYNC= 'name HIT_LIMIT 3';
```

Or combine it with signal and/or wait:

```sql
SET DEBUG_SYNC= 'name SIGNAL sig EXECUTE 2 HIT_LIMIT 3';
```

Here the first two hits emit the signal, the third hit returns the error
message and kills the query.

For cases where you are not sure that an action is taken and thus
cleared in any case, you can force to clear (deactivate) a sync point:

```sql
SET DEBUG_SYNC= 'name CLEAR';
```

If you want to clear all actions and clear the global signal, use:

```sql
SET DEBUG_SYNC= 'RESET';
```

This is the only way to reset the global signal to an empty string.

For testing of the facility itself you can execute a sync point just
as if it had been hit:

```sql
SET DEBUG_SYNC= 'name TEST';
```

### Formal Syntax

The string to "assign" to the DEBUG_SYNC variable can contain:

```sql
RESET |
<sync point name> TEST |
<sync point name> CLEAR |
<sync point name> {{SIGNAL <signal name> |
                   WAIT_FOR <signal name> [TIMEOUT <seconds>]}
                   [EXECUTE <count>] &| HIT_LIMIT <count>}
```

Here '&amp;|' means 'and/or'. This means that one of the sections
separated by '&amp;|' must be present or both of them.

### Activation/Deactivation

With a [MariaDB for debug build](/kb/en/compiling-mariadb-for-debugging/), it can be enabled by a mysqld command line option:

```sql
 --debug-sync-timeout[=default_wait_timeout_value_in_seconds]
```

<code class="highlight fixed" style="white-space:pre-wrap">'default_wait_timeout_value_in_seconds'</code> is the default timeout for the <code class="highlight fixed" style="white-space:pre-wrap">WAIT_FOR</code> action. If set to zero, the facility stays disabled.

The facility is enabled by default in the test suite, but can be
disabled with:

```sql
mysql-test-run.pl ... --debug-sync-timeout=0 ...
```

Likewise the default wait timeout can be set:

```sql
mysql-test-run.pl ... --debug-sync-timeout=10 ...
```

The command line option influences the readable value of the [debug_sync](/kb/en/server-system-variables/#debug_sync) system variable.

- If the facility is not compiled in, the system variable does not exist.

- If <code class="highlight fixed" style="white-space:pre-wrap">--debug-sync-timeout=0</code> the value of the variable reads as <code class="highlight fixed" style="white-space:pre-wrap">"OFF"</code>.

- Otherwise the value reads as <code class="highlight fixed" style="white-space:pre-wrap">"ON - current signal: "</code> followed by the current signal string, which can be empty.

The readable variable value is the same, regardless if read as a global
or session value.

Setting the [debug_sync](/kb/en/server-system-variables/#debug_sync)  system variable requires the <code class="highlight fixed" style="white-space:pre-wrap">'SUPER'</code> privilege.  You can never read back the
string that you assigned to the variable, unless you assign the value
that the variable already has. But that would give a parse
error. A syntactically correct string is parsed into a debug sync
action and stored apart from the variable value.

### Implementation

Pseudo code for a sync point:

```sql
#define DEBUG_SYNC(thd, sync_point_name)
        if (unlikely(opt_debug_sync_timeout))
          debug_sync(thd, STRING_WITH_LEN(sync_point_name))
```

The sync point performs a binary search in a sorted array of actions
for this thread.

The <code class="highlight fixed" style="white-space:pre-wrap">SET DEBUG_SYNC</code> statement adds a requested
action to the array or overwrites an existing action for the same sync
point. When it adds a new action, the array is sorted again.

### A typical synchronization pattern

There are quite a few places in MariaDB and MySQL where we use a
synchronization pattern like this:

```sql
mysql_mutex_lock(&mutex);
thd->enter_cond(&condition_variable, &mutex, new_message);
#if defined(ENABLE_DEBUG_SYNC)
if (!thd->killed && !end_of_wait_condition)
   DEBUG_SYNC(thd, "sync_point_name");
#endif
while (!thd->killed && !end_of_wait_condition)
  mysql_cond_wait(&condition_variable, &mutex);
thd->exit_cond(old_message);
```

Here are some explanations:

<code class="highlight fixed" style="white-space:pre-wrap">thd-&gt;enter_cond()</code> is used to register the condition
variable and the mutex in <code class="highlight fixed" style="white-space:pre-wrap">thd-&gt;mysys_var</code>. This is
done to allow the thread to be interrupted (killed) from its
sleep. Another thread can find the condition variable to signal and
mutex to use for synchronization in this thread's <code class="highlight fixed" style="white-space:pre-wrap">THD::mysys_var</code>.

<code class="highlight fixed" style="white-space:pre-wrap">thd-&gt;enter_cond()</code> requires the mutex to be acquired
in advance.

<code class="highlight fixed" style="white-space:pre-wrap">thd-&gt;exit_cond()</code> unregisters the condition variable
and mutex and releases the mutex.

If you want to have a Debug Sync point with the wait, please place it
behind <code class="highlight fixed" style="white-space:pre-wrap">enter_cond()</code>. Only then you can safely
decide, if the wait will be taken. Also you will have
<code class="highlight fixed" style="white-space:pre-wrap">THD::proc_info</code> correct when the sync point emits a
signal. <code class="highlight fixed" style="white-space:pre-wrap">DEBUG_SYNC</code> sets its own proc_info, but
restores the previous one before releasing its internal mutex. As soon
as another thread sees the signal, it does also see the proc_info from
before entering the sync point. In this case it will be "new_message",
which is associated with the wait that is to be synchronized.

In the example above, the wait condition is repeated before the sync
point. This is done to skip the sync point, if no wait takes place.
The sync point is before the loop (not inside the loop) to have it hit
once only. It is possible that the condition variable is signaled
multiple times without the wait condition to be true.

A bit off-topic: At some places, the loop is taken around the whole
synchronization pattern:

```sql
while (!thd->killed && !end_of_wait_condition)
{
  mysql_mutex_lock(&mutex);
  thd->enter_cond(&condition_variable, &mutex, new_message);
  if (!thd->killed [&& !end_of_wait_condition])
  {
    [DEBUG_SYNC(thd, "sync_point_name");]
    mysql_cond_wait(&condition_variable, &mutex);
  }
  thd->exit_cond(old_message);
}
```

Note that it is important to repeat the test for thd-&gt;killed after
<code class="highlight fixed" style="white-space:pre-wrap">enter_cond()</code>. Otherwise the killing thread may kill
this thread after it tested <code class="highlight fixed" style="white-space:pre-wrap">thd-&gt;killed</code> in the loop
condition and before it registered the condition variable and mutex in
<code class="highlight fixed" style="white-space:pre-wrap">enter_cond()</code>. In this case, the killing thread does
not know that this thread is going to wait on a condition variable. It
would just set <code class="highlight fixed" style="white-space:pre-wrap">THD::killed</code>. But if we would not
test it again, we would go asleep though we are killed. If the killing
thread would kill us when we are after the second test, but still
before sleeping, we hold the mutex, which is registered in mysys_var.
The killing thread would try to acquire the mutex before signaling the
condition variable. Since the mutex is only released implicitly in
<code class="highlight fixed" style="white-space:pre-wrap">mysql_cond_wait()</code>, the signaling happens at the
right place. We have a safe synchronization.

### Co-work with the DBUG facility

When running the MariaDB test suite with the
<code class="highlight fixed" style="white-space:pre-wrap">--debug-dbug</code> command line option, the Debug Sync
Facility writes trace messages to the DBUG trace. The following shell
commands proved very useful in extracting relevant information:

```sql
egrep 'query:|debug_sync_exec:' mysql-test/var/log/mysqld.1.trace
```

It shows all executed SQL statements and all actions executed by
synchronization points.

Sometimes it is also useful to see, which synchronization points have
been run through (hit) with or without executing actions. Then add
<code class="highlight fixed" style="white-space:pre-wrap">"|debug_sync_point:"</code> to the egrep pattern.