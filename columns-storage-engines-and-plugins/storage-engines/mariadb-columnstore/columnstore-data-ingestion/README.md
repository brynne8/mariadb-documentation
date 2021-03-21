# ColumnStore Data Ingestion

ColumnStore provides several mechanisms to ingest data:

- [cpimport](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-data-loading) provides the fastest performance for inserting data and ability to route data to particular PM nodes. <strong> Normally this should be the default choice for loading data </strong>.
- [LOAD DATA INFILE](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-sql-structure-and-commands/columnstore-data-manipulation-statements/columnstore-load-data-infile) provides another means of bulk inserting data. 
<ul start="1"><li>By default with autocommit on it will internally stream the data to an instance of the cpimport process. This requires some memory overhead on the UM server so may be less reliable than cpimport for very large imports.
</li><li>In transactional mode DML inserts are performed which will be significantly slower plus it will consume both binlog transaction files and ColumnStore VersionBuffer files.
</li></ul>
- DML, i.e. INSERT, UPDATE, and DELETE, provide row level changes. ColumnStore is optimized towards bulk modifications and so these operations are slower than they would be in say InnoDB.
<ul start="1"><li>Currently ColumnStore does not support operating as a replication slave target. 
</li><li>Bulk DML operations will in general perform better than multiple individual statements.
<ul start="1"><li>[INSERT INTO SELECT](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-batch-insert-mode) with autocommit behaves similarly to LOAD DATE INFILE in that internally it is mapped to cpimport for higher performance.
</li><li>Bulk update operations based on a join with a small staging table can be relatively fast especially if updating a single column.
</li></ul>
</li></ul>
- Using [ColumnStore Bulk Write SDK](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk) or [ColumnStore Streaming Data Adapters](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-streaming-data-adapters)

- [ColumnStore Bulk Data Loading](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-data-loading/) — Using high-speed bulk load utility cpimport
- [ColumnStore LOAD DATA INFILE](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-sql-structure-and-commands/columnstore-data-manipulation-statements/columnstore-load-data-infile/) — Using the LOAD DATA INFILE statement for bulk data loading.
- [ColumnStore Batch Insert Mode](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-batch-insert-mode/) — Batch data insert mode with cpimport
- [ColumnStore Bulk Write SDK](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk/) — Starting with MariaDB ColumnStore 1.4, the Bulk Write SDK is deprecated, a...
- [ColumnStore remote bulk data import: mcsimport](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-remote-bulk-data-import-mcsimport/) — Overview
mcsimport is a high-speed bulk load utility that imports data int...
- [ColumnStore Streaming Data Adapters](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-streaming-data-adapters/) — The ColumnStore Bulk Data API enables the creation of higher performance a...