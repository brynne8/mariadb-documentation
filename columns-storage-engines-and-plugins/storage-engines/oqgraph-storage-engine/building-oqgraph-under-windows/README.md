# Building OQGRAPH Under Windows

OQGRAPH v3 can be built on Windows.

##### MariaDB starting with [10.0.11](/kb/en/mariadb-10011-release-notes/)

This has been tested using Windows 7, Microsoft Visual Studio Express 2010 (32-bit), Microsoft Windows 64-bit Platform SDK 7.1 (64-bit), the Boost library &gt;= 1.55 and Judy 1.0.5.  Probably other recent versions of Boost, Judy or MSVC may work but these combinations have not been tested.

- Download the source package for Boost 1.55 from the Boost project website, [http://www.boost.org](http://www.boost.org)
- Download the source package for Judy 1.05 via [http://judy.sourceforge.net/](http://judy.sourceforge.net/)
- Follow the documented instructions for building under Windows from the command line: [Building_MariaDB_on_Windows](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/Building_MariaDB_on_Windows/)
<ul start="1"><li>Ensure that the following variable is set to CMAKE: `JUDY_ROOT=path\to\judy\unzipped`
</li><li>See also comments in `storage/oqgraph/cmake/FindJudy.cmake`</li></ul>