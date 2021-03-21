# InnoDB Page Compression

##### MariaDB starting with [10.1](/kb/en/what-is-mariadb-101/)

[InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) Page Compression was added in [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/). Support for the <a undefined>snappy</a> compression algorithm was added in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/).

## Overview

InnoDB page compression provides a way to compress InnoDB tables.

## Use Cases

- InnoDB page compression can be used on any storage device and any file system.

- InnoDB page compression is most efficient on file systems that support sparse files. See [Saving Storage Space with Sparse Files](#saving-storage-space-with-sparse-files) for more information.

- InnoDB page compression is most beneficial on solid state drives (SSDs) and other flash storage. See [Optimized for Flash Storage](#optimized-for-flash-storage) for more information.

- InnoDB page compression performs best when your storage device and file system support atomic writes, since that allows the [InnoDB doublewrite buffer](/kb/en/xtradbinnodb-doublewrite-buffer/) to be disabled. See [Atomic Write Support](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-performance-advanced-configurations/atomic-write-support/) for more information.

## Comparison with the `COMPRESSED` Row Format

InnoDB page compression is a modern way to compress your InnoDB tables. It is similar to InnoDB's [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format/) row format, but it has many advantages. Some of the differences are:

- With InnoDB page compression, compressed pages are immediately decompressed after being read from the tablespace file, and only uncompressed pages are stored in the buffer pool. In contrast, with InnoDB's [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format/) row format, compressed pages are decompressed immediately after they are read from the tablespace file, and both the uncompressed and compressed pages are stored in the buffer pool. This means that the [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format/) row format uses more space in the buffer pool than InnoDB page compression does.
- With InnoDB page compression, pages are compressed just before being written to the tablespace file. In contrast, with InnoDB's [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format/) row format, pages are re-compressed immediately after any changes, and the compressed pages are stored in the buffer pool alongside the uncompressed pages. These changes are then occasionally flushed to disk. This means that the [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format/) row format re-compresses data more frequently than InnoDB page compression does.
- With InnoDB page compression, multiple compression algorithms are supported. In contrast, with InnoDB's [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format/) row format, <a undefined>zlib</a> is the only supported compression algorithm. This means that the [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format/) row format has less compression options than InnoDB page compression does.

In general, InnoDB page compression is superior to the [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format/) row format.

## Comparison with Storage Engine-Independent Column Compression

- See [Storage Engine-Independent Column Compression - Comparison with InnoDB Page Compression](/kb/en/storage-engine-independent-column-compression/#comparison-with-innodb-page-compression).

## Configuring the InnoDB Page Compression Algorithm

There is not currently a table option to set different InnoDB page compression algorithms for individual tables.

However, the server-wide InnoDB page compression algorithm can be configured by setting the
<a undefined>innodb_compression_algorithm</a> system variable.

When this system variable is changed, the InnoDB page compression algorithm does not change for existing pages that were already compressed with a different InnoDB page compression algorithm. InnoDB is able to handle this situation without issues, because every page in an InnoDB tablespace contains metadata about the InnoDB page compression algorithm in the page header. This means that InnoDB supports having uncompressed pages and pages compressed with different InnoDB page compression algorithms in the same InnoDB tablespace at the same time.

This system variable can be set to one of the following values:

<table><tbody><tr><th>System Variable Value</th><th>Description</th></tr>
<tr><td><code>none</code></td><td>Pages are not compressed. This is the default value in <a href="/kb/en/mariadb-1023-release-notes/">MariaDB 10.2.3</a> and before, and <a href="/kb/en/mariadb-10121-release-notes/">MariaDB 10.1.21</a> and before.</td></tr>
<tr><td><code>zlib</code></td><td>Pages are compressed using the bundled <code><a href="https://www.zlib.net/">zlib</a></code> compression algorithm. This is the default value in <a href="/kb/en/mariadb-1024-release-notes/">MariaDB 10.2.4</a> and later, and <a href="/kb/en/mariadb-10122-release-notes/">MariaDB 10.1.22</a> and later.</td></tr>
<tr><td><code>lz4</code></td><td>Pages are compressed using the <code><a href="https://code.google.com/p/lz4/">lz4</a></code> compression algorithm.</td></tr>
<tr><td><code>lzo</code></td><td>Pages are compressed using the <code><a href="http://www.oberhumer.com/opensource/lzo/">lzo</a></code> compression algorithm.</td></tr>
<tr><td><code>lzma</code></td><td>Pages are compressed using the <code><a href="http://tukaani.org/xz/">lzma</a></code> compression algorithm.</td></tr>
<tr><td><code>bzip2</code></td><td>Pages are compressed using the <code><a href="http://www.bzip.org/">bzip2</a></code> compression algorithm.</td></tr>
<tr><td><code>snappy</code></td><td>Pages are compressed using the <code><a href="http://google.github.io/snappy/">snappy</a></code> algorithm.</td></tr>
</tbody></table>

However, on many distributions, the standard MariaDB builds do not support all InnoDB page compression algorithms by default.

This system variable can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL innodb_compression_algorithm='lzma';
```

This system variable can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
innodb_compression_algorithm=lzma
```

### Checking Supported InnoDB Page Compression Algorithms

On many distributions, the standard MariaDB builds do not support all InnoDB page compression algorithms by default. Therefore, if you want to use a specific InnoDB page compression algorithm, then you should check whether your MariaDB build supports it.

The <a undefined>zlib</a> compression algorithm is always supported.

A MariaDB build's support for other InnoDB page compression algorithms can be checked by querying the following status variables with [SHOW GLOBAL STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status/):

<table><tbody><tr><th>Status Variable</th><th>Description</th></tr>
<tr><td><code><a href="/kb/en/innodb-server-status-variables/#innodb_have_lz4">Innodb_have_lz4</a></code></td><td>Whether InnoDB supports the <code><a href="https://code.google.com/p/lz4/">lz4</a></code> compression algorithm.</td></tr>
<tr><td><code><a href="/kb/en/innodb-server-status-variables/#innodb_have_lzo">Innodb_have_lzo</a></code></td><td>Whether InnoDB supports the <code><a href="http://www.oberhumer.com/opensource/lzo/">lzo</a></code> compression algorithm.</td></tr>
<tr><td><code><a href="/kb/en/innodb-server-status-variables/#innodb_have_lzma">Innodb_have_lzma</a></code></td><td>Whether InnoDB supports the <code><a href="http://tukaani.org/xz/">lzma</a></code> compression algorithm.</td></tr>
<tr><td><code><a href="/kb/en/innodb-server-status-variables/#innodb_have_bzip2">Innodb_have_bzip2</a></code></td><td>Whether InnoDB supports the <code><a href="http://www.bzip.org/">bzip2</a></code> compression algorithm.</td></tr>
<tr><td><code><a href="/kb/en/innodb-server-status-variables/#innodb_have_snappy">Innodb_have_snappy</a></code></td><td>Whether InnoDB supports the <code><a href="http://google.github.io/snappy/">snappy</a></code> compression algorithm.</td></tr>
</tbody></table>

For example:

```sql
SHOW GLOBAL STATUS WHERE Variable_name IN (
   'Innodb_have_lz4', 
   'Innodb_have_lzo', 
   'Innodb_have_lzma', 
   'Innodb_have_bzip2', 
   'Innodb_have_snappy'
);
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| Innodb_have_lz4    | OFF   |
| Innodb_have_lzo    | OFF   |
| Innodb_have_lzma   | ON    |
| Innodb_have_bzip2  | OFF   |
| Innodb_have_snappy | OFF   |
+--------------------+-------+
5 rows in set (0.001 sec)
```

### Adding Support for an InnoDB Page Compression Algorithm

On many distributions, the standard MariaDB builds do not support all InnoDB page compression algorithms by default. Therefore, if you want to use certain InnoDB page compression algorithms, then you may need to do the following:

- Download the package for the desired compression library from the above links.
- Install the package for the desired compression library.
- Compile MariaDB from the source distribution.

The general steps for compiling MariaDB are:

- Download and unpack the source code distribution:

```sql
wget https://downloads.mariadb.com/MariaDB/mariadb-10.4.8/source/mariadb-10.4.8.tar.gz
tar -xvzf mariadb-10.4.8.tar.gz
cd mariadb-10.4.8/
```

- Configure the build using <a undefined>cmake</a>:

```sql
cmake .
```

- Check <a undefined>CMakeCache.txt</a> to confirm that it has found the desired compression library on your system.

- Compile the build:

```sql
make
```

- Either install the build:

```sql
make install
```

Or make a package to install:

```sql
make package
```

See [Compiling MariaDB From Source](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/) for more information.

## Enabling InnoDB Page Compression

InnoDB page compression is not enabled by default. However, InnoDB page compression can be enabled for just individual InnoDB tables or it can be enabled for all new InnoDB tables by default.

InnoDB page compression is also only supported if the InnoDB table is in a [file per-table](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/) tablespace. Therefore, the <a undefined>innodb_file_per_table</a> system variable must be set to `ON` to use InnoDB page compression.

InnoDB page compression is only supported if the InnoDB table uses the `Barracuda` [file format](/kb/en/xtradbinnodb-file-format/).Therefore, in [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, the <a undefined>innodb_file_format</a> system variable must be set to `Barracuda` to use InnoDB page compression.

InnoDB page compression is also only supported if the InnoDB table's [row format](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-row-formats-overview/) is [COMPACT](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compact-row-format/) or [DYNAMIC](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-dynamic-row-format/).

### Enabling InnoDB Page Compression by Default

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

The <a undefined>innodb_compression_default</a> system variable was first added in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/).

In [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/) and later, InnoDB page compression can be enabled for all new InnoDB tables by default by setting the <a undefined>innodb_compression_default</a> system variable to `ON`.

This system variable can be set to one of the following values:

<table><tbody><tr><th>System Variable Value</th><th>Description</th></tr>
<tr><td><code>OFF</code></td><td>New InnoDB tables do not use InnoDB page compression. This is the default value.</td></tr>
<tr><td><code>ON</code></td><td>New InnoDB tables use InnoDB page compression.</td></tr>
</tbody></table>

This system variable can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL innodb_compression_default=ON;
```

This system variable's session value can be changed dynamically with <a undefined>SET SESSION</a>. For example:

```sql
SET GLOBAL innodb_file_per_table=ON;

SET GLOBAL innodb_file_format='Barracuda';

SET GLOBAL innodb_default_row_format='dynamic';

SET GLOBAL innodb_compression_algorithm='lzma';

SET SESSION  innodb_compression_default=ON;

CREATE TABLE users (
   user_id int not null, 
   b varchar(200), 
   primary key(user_id)
) 
   ENGINE=InnoDB;
```

This system variable can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
innodb_compression_default=ON
```

### Enabling InnoDB Page Compression for Individual Tables

InnoDB page compression can be enabled for individual tables by setting the <a undefined>PAGE_COMPRESSED</a> table option to `1`. For example:

```sql
SET GLOBAL innodb_file_per_table=ON;

SET GLOBAL innodb_file_format='Barracuda';

SET GLOBAL innodb_default_row_format='dynamic';

SET GLOBAL innodb_compression_algorithm='lzma';

CREATE TABLE users (
   user_id int not null, 
   b varchar(200), 
   primary key(user_id)
) 
   ENGINE=InnoDB
   PAGE_COMPRESSED=1;
```

## Configuring the Compression Level

Some InnoDB page compression algorithms support a compression level option, which configures how the InnoDB page compression algorithm will balance speed and compression.

The compression level's supported values range from `1` to `9`. The range goes from the fastest to the most compact, which means that `1` is the fastest and `9` is the most compact.

Only the following InnoDB page compression algorithms currently support compression levels:

- <a undefined>zlib</a>
- <a undefined>lzma</a>

If an InnoDB page compression algorithm does not support compression levels, then it ignores any provided compression level value.

### Configuring the Default Compression Level

The default compression level can be configured by setting the
<a undefined>innodb_compression_level</a> system variable.

This system variable's default value is `6`.

This system variable can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL innodb_compression_level=9;
```

This system variable can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
innodb_compression_level=9
```

### Configuring the Compression Level for Individual Tables

The compression level for individual tables can also be configured by setting the <a undefined>PAGE_COMPRESSION_LEVEL</a> table option for the table. For example:

```sql
SET GLOBAL innodb_file_per_table=ON;

SET GLOBAL innodb_file_format='Barracuda';

SET GLOBAL innodb_default_row_format='dynamic';

SET GLOBAL innodb_compression_algorithm='lzma';

CREATE TABLE users (
   user_id int not null, 
   b varchar(200), 
   primary key(user_id)
) 
   ENGINE=InnoDB
   PAGE_COMPRESSED=1
   PAGE_COMPRESSION_LEVEL=9;
```

## Configuring the Failure Threshold and Maximum Padding

InnoDB page compression can encounter compression failures.

InnoDB page compression's failure threshold can be configured. If InnoDB encounters more compression failures than the failure threshold, then it pads pages with zeroed out bytes before attempting to compress them as a way to reduce failures. If the failure rate stays above the failure threshold, then InnoDB pads pages with more zeroed out bytes in 128 byte increments.

InnoDB page compression's maximum padding can also be configured.

### Configuring the Failure Threshold

The failure threshold can be configured by setting the <a undefined>innodb_compression_failure_threshold_pct</a> system variable.

This system variable's supported values range from `0` to `100`.

This system variable's default value is `5`.

This system variable can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL innodb_compression_failure_threshold_pct=10;
```

This system variable can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
innodb_compression_failure_threshold_pct=10
```

### Configuring the Maximum Padding

The maximum padding can be configured by setting the <a undefined>innodb_compression_pad_pct_max</a> system variable.

This system variable's supported values range from `0` to `75`.

This system variable's default value is `50`.

This system variable can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL innodb_compression_pad_pct_max=75;
```

This system variable can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
innodb_compression_pad_pct_max=75
```

## Saving Storage Space with Sparse Files

When InnoDB page compression is used, InnoDB may still write the compressed page to the tablespace file with the original size of the uncompressed page, which would be equivalent to the value of the <a undefined>innodb_page_size</a> system variable. This is done by design, because when InnoDB's I/O code needs to read the page from disk, it can only read the full page size. However, this is obviously not optimal.

On file systems that support sparse files, this problem is solved by writing the tablespace file as a sparse file using the <em>punch hole</em> technique. With the <em>punch hole</em> technique, InnoDB will only write the actual compressed page size to the tablespace file, aligned to sector size. The rest of the page is trimmed.

This <em>punch hole</em> technique allows InnoDB to read the compressed page from disk as the full page size, even though the compressed page really takes up less space on the file system.

There are some potential disadvantages to using sparse files:

- Some utilities may require special options in order to handle sparse files in an efficient manner.
- Most existing file systems are slow to <a undefined>unlink()</a> sparse files. As a consequence, if a tablespace file is a sparse file, then dropping the table can be very slow.

### Sparse File Support on Linux

On Linux, the following file systems support sparse files:

- `ext3`
- `ext4`
- `xfs`
- `btrfs`
- `nvmfs`

On Linux, file systems need to support the <a undefined>fallocate()</a> system call with the `FALLOC_FL_PUNCH_HOLE` and `FALLOC_FL_KEEP_SIZE` flags. For example:

```sql
fallocate(file_handle, FALLOC_FL_PUNCH_HOLE | FALLOC_FL_KEEP_SIZE, file_offset, remainder_len);
```

Some Linux utilities may require special options in order to work with sparse files efficiently. For example:

- The <a undefined>ls</a> utility will report the non-sparse size of the tablespace file when executed with default behavior, but <a undefined>ls -s</a> will report the actual amount of storage allocated for the tablespace file.
- The <a undefined>cp</a> utility is pretty good at auto-detecting sparse files, but it also provides the <a undefined>cp --sparse=always</a> and <a undefined>cp --sparse=never</a> options, if the auto-detection is not desired.
- The <a undefined>tar</a> utility will archive sparse files with their non-sparse size when executed with default behavior, but <a undefined>tar --sparse</a> will auto-detect sparse files, and archive them with their sparse size.

### Sparse File Support on Windows

On Windows, the following file systems support sparse files:

- `NTFS`

On Windows, file systems need to support the <a undefined>DeviceIoControl()</a> function with the <a undefined>FSCTL_SET_SPARSE</a> and <a undefined>FSCTL_SET_ZERO_DATA</a> control codes. For example:

```sql
DeviceIoControl(file_handle, FSCTL_SET_SPARSE, inbuf, inbuf_size, 
   outbuf, outbuf_size,  NULL, &overlapped)
...
DeviceIoControl(file_handle, FSCTL_SET_ZERO_DATA, inbuf, inbuf_size, 
   outbuf, outbuf_size,  NULL, &overlapped)
```

### Configuring InnoDB to use Sparse Files

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and later, InnoDB uses the <em>punch hole</em> technique to create sparse files used automatically when the underlying file system supports sparse files.

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and before, InnoDB can be configured to use the <em>punch hole</em> technique to create sparse files by configuring the <a undefined>innodb_use_trim</a> and <a undefined>innodb_use_fallocate</a> system variables. These system variables can be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
innodb_use_trim=ON
innodb_use_fallocate=ON
```

## Optimized for Flash Storage

InnoDB page compression was designed to be optimized on solid state drives (SSDs) and other flash storage.

InnoDB page compression was originally developed by collaborating with [Fusion-io](http://fusionio.com). As a consequence, it was originally designed to work best on [FusionIO devices](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-performance-advanced-configurations/fusion-io/fusion-io-introduction/) using [NVMFS](https://ieeexplore.ieee.org/document/6558434). [Fusion-io](http://fusionio.com) has since been acquired by [Western Digital](https://www.westerndigital.com/), and they have decided not to continue supporting [NVMFS](https://ieeexplore.ieee.org/document/6558434).

However, InnoDB page compression is still likely to be most optimized on solid state drives (SSDs) and other flash storage.

InnoDB page compression works without any issues on hard disk drives (HDDs). However, since its compression relies on the use of sparse files, the data may be somewhat fragmented on disk. This fragmentation may hurt performance on HDDs, since they handle random reads and writes much more slowly than flash storage.

## Configuring InnoDB Page Flushing

With InnoDB page compression, pages are compressed when they are flushed to disk. Therefore, it can be helpful to optimize the configuration of InnoDB's page flushing. See [InnoDB Page Flushing](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-page-flushing/) for more information.

## Monitoring InnoDB Page Compression

InnoDB page compression can be monitored by querying the following status variables with [SHOW GLOBAL STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status/):

<table><tbody><tr><th>Status Variable</th><th>Description</th></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_page_compression_saved">Innodb_page_compression_saved</a></code></td><td>Bytes saved by compression</td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_page_compression_trim_sect512">Innodb_page_compression_trim_sect512</a></code></td><td>Number of 512 sectors trimmed</td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_page_compression_trim_sect1024">Innodb_page_compression_trim_sect1024</a></code></td><td>Number of 1024 sectors trimmed</td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_page_compression_trim_sect2048">Innodb_page_compression_trim_sect2048</a></code></td><td>Number of 2048 sectors trimmed</td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_page_compression_trim_sect4096">Innodb_page_compression_trim_sect4096</a></code></td><td>Number of 4096 sectors trimmed</td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_page_compression_trim_sect8192">Innodb_page_compression_trim_sect8192</a></code></td><td>Number of 8192 sectors trimmed</td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_page_compression_trim_sect16384">Innodb_page_compression_trim_sect16384</a></code></td><td>Number of 16384 sectors trimmed</td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_page_compression_trim_sect32768">Innodb_page_compression_trim_sect32768</a></code></td><td>Number of 32768 sectors trimmed</td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_num_pages_page_compressed">Innodb_num_pages_page_compressed</a></code></td><td>Number of pages compressed</td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_num_page_compressed_trim_op">Innodb_num_page_compressed_trim_op</a></code></td><td>Number of trim operations</td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_num_page_compressed_trim_op_saved">Innodb_num_page_compressed_trim_op_saved</a></code></td><td>Number of trim operations saved</td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_num_pages_page_decompressed">Innodb_num_pages_page_decompressed</a></code></td><td>Number of pages decompressed</td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_num_pages_page_compression_error">Innodb_num_pages_page_compression_error</a></code></td><td>Number of compression errors</td></tr>
</tbody></table>

With InnoDB page compression, a page is only compressed when it is flushed to disk. This means that if you are monitoring InnoDB page compression via these status variables, then the status variables values will only get incremented when the dirty pages are flushed to disk, which does not necessarily happen immediately. For example:

```sql
CREATE TABLE `tab` (
     `id` int(11) NOT NULL,
     `str` varchar(50) DEFAULT NULL,
     PRIMARY KEY (`id`)
   ) ENGINE=InnoDB;
 
INSERT INTO tab VALUES (1, 'str1');

SHOW GLOBAL STATUS LIKE 'Innodb_num_pages_page_compressed';
+----------------------------------+-------+
| Variable_name                    | Value |
+----------------------------------+-------+
| Innodb_num_pages_page_compressed | 0     |
+----------------------------------+-------+
 
SET GLOBAL innodb_file_per_table=ON;

SET GLOBAL innodb_file_format='Barracuda';

SET GLOBAL innodb_default_row_format='dynamic';

SET GLOBAL innodb_compression_algorithm='lzma';
 
ALTER TABLE tab PAGE_COMPRESSED=1;

SHOW GLOBAL STATUS LIKE 'Innodb_num_pages_page_compressed';
+----------------------------------+-------+
| Variable_name                    | Value |
+----------------------------------+-------+
| Innodb_num_pages_page_compressed | 0     |
+----------------------------------+-------+
 
SELECT SLEEP(10);
+-----------+
| SLEEP(10) |
+-----------+
|         0 |
+-----------+
 
SHOW GLOBAL STATUS LIKE 'Innodb_num_pages_page_compressed';
+----------------------------------+-------+
| Variable_name                    | Value |
+----------------------------------+-------+
| Innodb_num_pages_page_compressed | 3     |
+----------------------------------+-------+
```

## Compatibility with Backup Tools

[Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/) supports InnoDB page compression.

[Percona XtraBackup](/kb/en/percona-xtrabackup/) does not support InnoDB page compression.

## Acknowledgements

- InnoDB page compression was developed by collaborating with [Fusion-io](http://fusionio.com). Special thanks especially to Dhananjoy Das and Torben Mathiasen.

## See Also

- [Storage-Engine Independent Column Compression](/replication/optimization-and-tuning/optimization-and-tuning-compression/storage-engine-independent-column-compression/)
- [Atomic Write Support](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-performance-advanced-configurations/atomic-write-support/)
- [MariaDB Introduces Atomic Writes](https://blog.mariadb.org/mariadb-introduces-atomic-writes/)
- [Small Datum: Third day with InnoDB transparent page compression](http://smalldatum.blogspot.com/2015/09/third-day-with-innodb-transparent-page.html)
- [InnoDB holepunch compression vs the filesystem in MariaDB 10.1](https://blog.mariadb.org/innodb-holepunch-compression-vs-the-filesystem-in-mariadb-10-1/)
- [Significant performance boost with new MariaDB page compression on FusionIO](https://blog.mariadb.org/significant-performance-boost-with-new-mariadb-page-compression-on-fusionio/)
- [INFLOW '14: NVM Compression—Hybrid Flash-Aware Application Level Compression](https://www.usenix.org/conference/inflow14/workshop-program/presentation/das)