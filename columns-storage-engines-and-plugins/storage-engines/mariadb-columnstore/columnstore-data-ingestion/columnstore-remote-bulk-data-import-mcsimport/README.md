# ColumnStore remote bulk data import: mcsimport

## Overview

mcsimport is a high-speed bulk load utility that imports data into ColumnStore tables in a fast and efficient manner utilizing ColumnStore's [Bulk Write SDK](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk/). Unlike cpimport, mcsimport was designed to be executed from a remote machine that doesn't necessarily needs to be a [UM](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-user-module/) or [PM](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-performance-module/). mcsimport is further executable from Windows and Linux operating systems.<br>
Similar to cpimport, mcsimport accepts as input any flat file that contains a delimiter between fields of data (i.e. columns in a table). The default delimiter is a comma (‘<strong>,</strong>’), but other delimiters such as pipes may also be used. By default mcsimport expects the data values to be in the same order as the create table statement, and a date format of ‘<em>YYYY-MM-DD HH:MM:SS</em>’. But, these settings can be overwritten in a mapping file which allows  customizeable input column to ColumnStore column mappings, the usage of individual input column specific date formats utilizing the [strptime](http://pubs.opengroup.org/onlinepubs/9699919799/functions/strptime.html) format, and the specification of default values for non mapped target columns.

It is important to note that:

- The bulk loads are an append operation to a table so they allow existing data to be read and remain unaffected during the process.
- The bulk loads do not write their data operations to the transaction log; they are not transactional in nature but are considered an atomic operation at this time. Information markers, however, are placed in the transaction log so the DBA is aware that a bulk operation did occur.
- Upon completion of the load operation, a high water mark in each column file is moved in an atomic operation that allows for any subsequent queries to read the newly loaded data. This append operation provides for consistent read but does not incur the overhead of logging the data.

There are three primary steps to using the mcsimport utility:

1 Create the Columnstore.xml configuration file that holds the information of the ColumnStore instance to connect to.
2 Optionally create a mapping file that defines the mapping between input file and target ColumnStore table.
3 Run the mcsimport utility to perform the data import.

## Installation

On Linux systems mcsimport requires the installation of the ColumnStore Bulk Write SDK, on Windows systems the Bulk Write SDK is bundled with mcsimport and doesn't require an extra installation.

### RHEL, CentOS, Debian / Ubuntu Repositories

mcsimport can also be installed from our MariaDB ColumnStore Tools repository. Detailed information can be found [here](/kb/en/installing-mariadb-ax-mariadb-columnstore-from-the-package-repositories-122/#mariadb-columnstore-tools-package).

### RHEL / CentOS 7 Package

First, install the Bulk Write SDK and dependencies according to following [documentation](/kb/en/columnstore-bulk-write-sdk/#rhel-centos-7-package).

Afterwards, you can install mcsimport via:

```sql
sudo rpm -ivh mariadb-columnstore-tools*.rpm
```

### Ubuntu 16 / Debian 9 Package

First, install the Bulk Write SDK and dependencies according to following [documentation](/kb/en/columnstore-bulk-write-sdk/#ubuntu-16-debian-9-package).

Afterwards, you can install mcsimport via:

```sql
sudo dpkg -i mariadb-columnstore-tools*.deb
```

### Debian 8 Package

First, install the Bulk Write SDK and dependencies according to following [documentation](/kb/en/columnstore-bulk-write-sdk/#debian-8-package).

Afterwards, you can install mcsimport via:

```sql
sudo dpkg -i mariadb-columnstore-tools*.deb
```

### Windows 10 Package

To install mcsimport on Windows 10 you simply have to follow the installation wizard of the installer.

[http://downloads.mariadb.com/ColumnStore-Tools/latest/winx64-packages/](http://downloads.mariadb.com/ColumnStore-Tools/latest/winx64-packages/)

### ColumnStore server configuration

As mcsimport is using the [Bulk Write SDK](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk/) for the injection, all ports required by the ColumnStore Bulk write SDK need to be accessible from the client executing mcsimport at the target ColumnStore server. These are in particular the TCP ports 8616, 8630, and 8800.

## Syntax

```sql
mcsimport database table input_file [-m mapping_file] [-c Columnstore.xml] [-d delimiter]
[-n null_option] [-df date_format] [-default_non_mapped] [-E enclose_by_character] 
[-C escape_character] [-rc read_cache_size] [-header] [-ignore_malformed_csv] [-err_log]
```

### -m mapping_file

The mapping file is used to define the mapping between source csv columns and target ColumnStore columns, to define column specific input date formats, and to set default values for ignored target columns. It follows the Yaml 1.2 standard and can address the source csv columns implicit and explicit. <br>
Source csv columns can only be identified by their position in the csv file starting with 0, and target ColumnStore columns can be identified either by their position or name.

Following snippet is an example for an implicit mapping file.

```sql
- column:
  target: 0
- column:
  - ignore
- column:
  target: id
- column:
  target: occurred
  format: "%d %b %Y %H:%M:%S"
- target: 2
  value: default
- target: salary
  value: 20000
```

It defines that the first csv column (#0) is mapped to the first column in the ColumnStore table, that the second csv column (#1) is ignored and won't be injected into the target table, that the third csv column (#2) is mapped to the ColumnStore column with the name id, and that the fourth csv column (#3) is mapped to the ColumnStore column with the name <em>occurred</em> and uses a specific date format. (defined using the [strptime](http://pubs.opengroup.org/onlinepubs/9699919799/functions/strptime.html) format) The mapping file further defines that for the third ColumnStore target column (#2) its default value will be used, and that the ColumnStore target column with the name <em>salary</em> will be set to 20000 for all injections.

Explicit mapping is also possible.

```sql
- column: 0
  target: id
- column: 4
  target: salary
- target: timestamp
  value: 2018-09-13 12:00:00
```

Using this variant the first (#0) csv source column is mapped to the target ColumnStore column with the name <em>id</em>, and the fifth source csv column (#4) is mapped to the target ColumnStore column with the name <em>salary</em>. It further defines that the target ColumnStore column timestamp uses a default value of <em>2018-09-13 12:00:00</em> for the injection.

### -c Columnstore.xml

As mcsimport is built upon ColumnStore's  [Bulk Write SDK](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk/) it inherits its methods to connect to ColumnStore instances to ingest data. By default mcsimport uses the standard configuration file <em>/usr/local/mariadb/ColumnStore/etc/Columnstore.xml</em> or if set the one defined through the environment variable <em>COLUMNSTORE_INSTALL_DIR</em> to connect to the remote Columnstore instance. Individual configurations can be defined through the command line parameter -c. Instructions on how to prepare Columnstore.xml for remote ingestion can be found [here](/kb/en/columnstore-bulk-write-sdk/#environment-configuration).

### -d delimiter

The default delimiter of the CSV input file is a comma (‘<strong>,</strong>’) and can be changed through the command line parameter -d. Only one character delimiters are currently supported.

### -df date_format

By default mcsimport uses <em>YYYY-MM-DD HH:MM:SS</em> as input date format. An individual global date format can be specified via the command line parameter -df using the [strptime](http://pubs.opengroup.org/onlinepubs/9699919799/functions/strptime.html) format. Column specific input date formats can be defined in the mapping file and overwrite the global date format.

### -n null_option

By default mcsimport treats input strings with the value "NULL" as data. If the null_option is set to 1 strings with the value "NULL" are treated as <em>NULL</em> values.

### -default_non_mapped

mcsimport needs to inject values for all ColumnStore columns of the target table. In order to use the ColumnStore column's default values for all non mapped target columns the global parameter <em>default_non_mapped</em> can be used. Target column specific default values in the mapping file overwrite the global default values of this parameter.

### -E enclose_by_character

By default mcsimport uses the double-quote character <strong>"</strong> as enclosing character. It can be changed through the command line parameter -E. The enclosing character's length is limited to 1.

### -C escape_character

By default mcsimport uses the double-quote character <strong>"</strong> as escaping character. It can be changed through the command line parameter -C. The escaping character's length is limited to 1.

### -rc read_cache_size

By default mcsimport uses a read cache size of 20,971,520 (20 MiB) to cache chunks of the input file in RAM. It can be changed through the command line paramter -rc. A minimum cache size of 1,048,576 (1 MiB) is required.

### -header

Choose this flag to ignore the first line of the input CSV file as header. (It won't be injected)

### -ignore_malformed_csv

By default mcsimport rolls back the entire bulk import if a malformed csv entry is found. With this option mcsimport ignores detected malformed csv entries and continiues with the injection.

### -err_log

With this option an optional error log file is written which states truncated, saturated, and invalid values during the injection. If the command line parameter <em>-ignore_malformed_csv</em> is chosen, it also states which lines were ignored.