# MariaDB 5.2 Replication Feature Preview

<strong>Note:</strong> This page is obsolete. The information is old, outdated, or otherwise currently incorrect. We are keeping the page for historical reasons only. <strong>Do not</strong> rely on the information in this article.

This page describes a <em>"feature preview release"</em> which previewed some replication-related features which are included in [MariaDB 5.3](/kb/en/what-is-mariadb-53/). If you would like to try out the features mentioned here, it is recommended that you use [MariaDB 5.3](/kb/en/what-is-mariadb-53/) ([download MariaDB 5.3 here](http://downloads.askmonty.org/mariadb/5.3/)) instead of the actual release described below. Likewise, the code is available in the [MariaDB 5.3 tree on Launchpad](https://launchpad.net/maria/5.3).

## About this release

There has been
quite a lot of interest in these features, and providing this feature preview
release allows the developers to get more and earlier feedback, as well as
allowing more users an early opportunity to evaluate the new features.

This feature preview release is based on [MariaDB 5.2](/kb/en/what-is-mariadb-52/), adding a number of fairly
isolated features that are considered complete and fairly well-tested. It is
however not a stable or GA release, nor is it planned to be so.

The stable
release including these features will be <strong>[MariaDB 5.3](/kb/en/what-is-mariadb-53/)</strong>. That being said, we
greatly welcome any feedback / bug reports, and will strive to fix any issues
found and we will update the feature preview until [MariaDB 5.3](/kb/en/what-is-mariadb-53/) stable is ready.

## Download/Installation

These packages are generated the same way as "official" MariaDB
releases. Please see the [main download pages](/kb/en/downloads/) for more detailed
instructions on installation etc.

The instructions below use the mirror
[ftp.osuosl.org](http://ftp.osuosl.org/), but any of the MariaDB
mirrors can be used by replacing the appropriate part of the URLs. See the
[main download page](http://downloads.askmonty.org) for what
mirrors are available.

### Debian/Ubuntu

For Debian and Ubuntu, it is highly recommended to install from the
repositories, using `apt-get`, `aptitude`, or other favorite package
managers.

First import the [public key](http://ftp.osuosl.org/pub/mariadb/PublicKey) with
which the repositories are signed, so that `apt` can verify the integrity of
the packages it downloads. For example like this:

```sql
wget -O- http://ftp.osuosl.org/pub/mariadb/PublicKey | sudo apt-key add -
```

Now add the appropriate repository. An easy way is to create a file called
`mariadb-5.2-rpl.list` in `/etc/apt/sources.list.d/` with contents like
this for Debian:

```sql
deb http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/debian squeeze main
deb-src http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/debian squeeze main
```

Or this for Ubuntu:

```sql
deb http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/ubuntu maverick main
deb-src http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/ubuntu maverick main
```

Replace "squeeze" or "maverick" in the examples above with the appropriate
distribution name. Supported are "lenny" and "squeeze" for Debian, and
"hardy", "jaunty", "karmic", "lucid", and "maverick" for Ubuntu.

Now run

```sql
sudo apt-get update
```

The packages can now be installed with your package manager of choice, for
example:

```sql
sudo apt-get install mariadb-server-5.2
```

(To manually download and install packages, browse the directories below
[http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/](http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/) - the .debs are in
`debian/pool/` and `ubuntu/pool/`, respectively.)

### Generic Linux binary tarball

Generic linux binary tarballs can be downloaded here:

- i386 (32-bit): [http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/misc/kvm-bintar-hardy-x86/](http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/misc/kvm-bintar-hardy-x86/)
- amd64 (64-bit): [http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/misc/kvm-bintar-hardy-amd64/](http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/misc/kvm-bintar-hardy-amd64/)

### Centos 5 RPMs

- i386 (32-bit): [http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/misc/kvm-rpm-centos5-x86/](http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/misc/kvm-rpm-centos5-x86/)
- amd64 (64-bit): [http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/misc/kvm-rpm-centos5-amd64/](http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/misc/kvm-rpm-centos5-amd64/)

### Windows (32-bit)

- [http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/misc/kvm-zip-winxp-x86/](http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/misc/kvm-zip-winxp-x86/)

### Source tarball

- [http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/misc/kvm-tarbake-jaunty-x86/](http://ftp.osuosl.org/pub/mariadb/mariadb-5.2-rpl/misc/kvm-tarbake-jaunty-x86/)

### Launchpad bzr branch:

- [`lp:~maria-captains/maria/mariadb-5.2-rpl`](https://code.launchpad.net/~maria-captains/maria/mariadb-5.2-rpl)

## New Features in the [MariaDB 5.2](/kb/en/what-is-mariadb-52/) replication feature preview

Here is a summary of the new features included in this preview release. The
headings link to more detailed information.

### [Group commit for the binary log](/kb/en/group-commit/)

This preview release implements group commit which works when using XtraDB with
the binary log enabled. (In previous MariaDB releases, and all MySQL releases at
the time of writing, group commit works in InnoDB/XtraDB when the binary log
is disabled, but stops working when the binary log is enabled).

### [Enhancements for START TRANSACTION WITH CONSISTENT SNAPSHOT](/kb/en/enhancements-for-start-transaction-with-consistent/)

`START TRANSACTION WITH CONSISTENT SNAPSHOT` now also works with the binary
log. This means it is possible to obtain the binlog position corresponding
to a transactional snapshot of the database without blocking any other
queries. This is used by `mysqldump --single-transaction --master-data` to do
a fully non-blocking backup which can be used to provision a new slave.

`START TRANSACTION WITH CONSISTENT SNAPSHOT` now also works consistently
between transactions involving more than one storage engine (currently XTraDB
and PBXT support this).

### [Annotation of row-based replication events with the original SQL statement](/clients-utilities/mysqlbinlog/annotate_rows_log_event/)

When using row-based replication, the binary log does not contain SQL
statements, only discrete single-row insert/update/delete <em>events</em>. This can
make it harder to read mysqlbinlog output and understand where in an
application a given event may have originated, complicating analysis and
debugging.

This feature adds an option to include the original SQL statement as a
comment in the binary log (and shown in mysqlbinlog output) for row-based
replication events.

### [Row-based replication for tables with no primary key](/replication/standard-replication/row-based-replication-with-no-primary-key/)

This feature can improve the performance of row-based replication on tables
that do not have a primary key (or other unique key), but which do have another
index that can help locate rows to update or delete. With this feature, index
cardinality information from `ANALYZE TABLE` is considered when selecting the
index to use (before this feature is implemented, the first index was selected
unconditionally).

### [PBXT consistent commit ordering](/kb/en/enhancements-for-start-transaction-with-consistent/)

This feature implements the new commit ordering storage engine API in
PBXT. With this feature, it is possible to use <code>START TRANSACTION WITH
CONSISTENT SNAPSHOT</code> and get consistency among transactions which involve both
XtraDB and InnoDB. (Without this feature, there is no such consistency
guarantee. For example, even after running <code>START TRANSACTION WITH CONSISTENT
SNAPSHOT</code> it was still possible for the InnoDB/XtraDB part of some
transaction <em>T</em> to be visible and the PBXT part of the same transaction <em>T</em>
to not be visible.)

### Miscellaneous

- This preview also includes a small change to make mysqlbinlog omit
  redundant `use` statements around `BEGIN`, `SAVEPOINT`, `COMMIT`,
  and `ROLLBACK` events when reading MySQL 5.0 binlogs.
- The preview included a feature
  [--innodb-release-locks-early](/kb/en/innodb-release-locks-early/). However we
  decided to omit this feature from future MariaDB releases because of a
  fundamental design bug, [lp:798213](https://bugs.launchpad.net/maria/+bug/798213).