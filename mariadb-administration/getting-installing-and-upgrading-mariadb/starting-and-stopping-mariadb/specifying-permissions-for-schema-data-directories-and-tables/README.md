# Specifying Permissions for Schema (Data) Directories and Tables

## Default File Permissions

By default MariaDB uses the following permissions for files and directories:

<table><tbody><tr><th>Object Type</th><th>Default Mode</th><th>Default Permissions</th></tr>
<tr><td>Files</td><td><code>0660</code></td><td><code>-rw-rw----</code></td></tr>
<tr><td>Directories</td><td><code>0700</code></td><td><code>drwx------</code></td></tr>
</tbody></table>

## Configuring File Permissions with Environment Variables

You can configure MariaDB to use different permissions for files and directories by setting the following [environment variables](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-environment-variables) before you start the server:

<table><tbody><tr><th>Object Type</th><th>Environment Variable</th></tr>
<tr><td>Files</td><td><code>UMASK</code></td></tr>
<tr><td>Directories</td><td><code>UMASK_DIR</code></td></tr>
</tbody></table>

In other words, if you would run the following in a shell:

```sql
export UMASK=0640
export UMASK_DIR=0750
```

These environment variables do not set the umask. They set the default file system permissions. See [MDEV-23058](https://jira.mariadb.org/browse/MDEV-23058) for more information.

### Configuring File Permissions with systemd

If your server is started by [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd), then there is a specific way to configure the umask. See [Systemd: Configuring the umask](/kb/en/systemd/#configuring-the-umask) for more information.