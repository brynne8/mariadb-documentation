# Bolt Examples

This page shows some examples of what we can do with Bolt to administer a set of MariaDB servers. Bolt is a tool that is part of the [Puppet](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/automated-mariadb-deployment-and-administration-puppet-and-mariadb/) ecosystem.

For information about installing Bolt, see [Installing Bolt](https://puppet.com/docs/bolt/latest/bolt_installing.html) in Bolt documentation.

## Inventory Files

<br><br>
The simplest way to call Bolt and instruct it to do something on some remote targets is the following:
<br><br>

```sql
bolt ... --nodes 100.100.100.100,200.200.200.200,300,300,300,300
```

However, for non-trivial setups it is usually better to use an inventory file. An example:

```sql
targets:
  - uri: maria-1.example.com
    name: maria_1
    alias: mariadb_main
  ...
```

In this way, it will be possible to refer the target by name or alias.

We can also define groups, followed by the group members. For example:

```sql
groups:
  - name: mariadb-staging
    targets:
        - uri: maria-1.example.com
        name: maria_1
        - uri: maria-2.example.com
        name: maria_2
  - name: mariadb-production
    targets:
      ...
...
```

With an inventory of this type, it will be possible to run Bolt actions against all the targets that are members of a group:

```sql
bolt ... --nodes mariadb-staging
```

In the examples in the rest of the page, the `--targets` parameter will be indicated in this way, for simplicity: `--targets &lt;targets&gt;`.

## Running Commands on Targets

The simplest way to run a command remotely is the following:

```sql
bolt command run 'mariadb-admin start-all-slaves' --targets <targets>
```

## Copying Files

To copy a file or a whole directory to targets:

```sql
bolt file upload /path/to/source /path/to/destination --targets <targets>
```

To copy a file or a whole directory from the targets to the local host:

```sql
bolt file download /path/to/source /path/to/destination --targets <targets>
```

## Running Scripts on Targets

We can use Bolt to run a local script on remote targets. Bolt will temporarily copy the script to the targets, run it, and delete it from the targets. This is convenient for scripts that are meant to only run once.

```sql
bolt script run rotate_logs.sh --targets <targets>
```

## Running Tasks on Targets

Puppet tasks are not always as powerful as custom scripts, but they are simpler and many of them are idempotent. The following task stops MariaDB replication:

```sql
bolt task run mysql::sql --targets <targets> sql="STOP REPLICA"
```

## Applying Puppet Code on Targets

It is also possible to apply whole manifests or portions of Puppet code (resources) on the targets.

To apply a manifest:

```sql
bolt apply manifests/server.pp  --targets <targets>
```

To apply a resource description:

```sql
bolt apply --execute "file { '/etc/mysql/my.cnf': ensure => present }" --targets <targets>
```

## Bolt Resources and References

- [Bolt documentation](https://puppet.com/docs/bolt/latest/bolt.html).
- [Bolt on GitHub](https://github.com/puppetlabs/bolt).

Further information about the concepts explained in this page can be found in Bolt documentation:

- [Inventory Files](https://puppet.com/docs/bolt/latest/inventory_file_v2.html) in Bolt documentation.
- [Applying Puppet code](https://puppet.com/docs/bolt/latest/applying_manifest_blocks.html) in Bolt documentation.

---

Content initially contributed by [Vettabase Ltd](https://vettabase.com/).