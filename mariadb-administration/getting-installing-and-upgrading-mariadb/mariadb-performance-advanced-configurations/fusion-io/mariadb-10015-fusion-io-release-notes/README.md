# MariaDB 10.0.15 Fusion-io Release Notes

<strong>Note:</strong> This page describes features in an <strong><em>unreleased</em></strong> version of MariaDB.<br><br>

<strong><em>Unreleased</em></strong> means there are no official packages or
binaries available for download which contain the features. If you want to try out any of the new features described here you will
need to [get](/kb/en/getting-the-mariadb-source-code/) and [compile](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/) the
code yourself.

[Download](http://ftp.osuosl.org/pub/mariadb/mariadb-10.0.15-fusion-io/)
[Release Notes](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-performance-advanced-configurations/fusion-io/mariadb-10015-fusion-io-release-notes/)
[Changelog](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-performance-advanced-configurations/fusion-io/mariadb-10015-fusion-io-changelog/)
[Fusion-io Introduction](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-performance-advanced-configurations/fusion-io/fusion-io-introduction/)

<strong>Release date:</strong>  12 Dec 2014

<strong>For an overview of [MariaDB 10.0](/kb/en/what-is-mariadb-100/) Fusion-io see the
[Fusion-io Introduction](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-performance-advanced-configurations/fusion-io/fusion-io-introduction/) page.</strong>

Thanks, and enjoy MariaDB!

## Notable Changes

Since the [MariaDB 10.0.9 Fusion-io preview](https://blog.mariadb.org/significant-performance-boost-with-new-mariadb-page-compression-on-fusionio/) release, the following notable changes have been made.

- Merged with [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/) release
- Added support for 4K sector size if supported
<ul start="1"><li>Added status variables for 1K, 2K, 4K, 8K, 16K, and 32K trims
</li></ul>
- Added innodb-compression-algorithm configuration variable to select default compression method
- Added support for
<ul start="1"><li>LZO compression
</li><li>LZMA compression
</li><li>bzip2 compression
</li></ul>

## Other Changes

For a complete list of changes made in [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/), with links to detailed
information on each push, see the [changelog](/kb/en/mariadb-10015-changelog/).

---

Be notified of new MariaDB Server releases automatically by [subscribing](https://lists.askmonty.org/cgi-bin/mailman/listinfo/announce) to the MariaDB Foundation community announce 'at' mariadb.org announcement list (this is a low traffic, announce-only list). MariaDB Corporation customers will be notified for all new releases, security issues and critical bug fixes for all MariaDB Corporation products thanks to the Notification Services.
<br><br>
MariaDB may already be included in your favorite OS distribution. More
information can be found on the
[Distributions which Include MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/distributions-which-include-mariadb/)
page.