# CONNECT System Variables

This page documents system variables related to the [CONNECT storage engine](/columns-storage-engines-and-plugins/storage-engines/connect/). See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for a complete list of system variables and instructions on setting them.

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

#### `connect_class_path`

- <strong>Description:</strong> Java class path
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-class-path=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong>
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong>

---

#### `connect_cond_push`

- <strong>Description:</strong> Enable condition pushdown
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-cond-push={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/), [MariaDB 10.2.14](/kb/en/mariadb-10214-release-notes/), [MariaDB 10.1.33](/kb/en/mariadb-10133-release-notes/), [MariaDB 10.0.35](/kb/en/mariadb-10035-release-notes/)

---

#### `connect_conv_size`

- <strong>Description:</strong> The size of the [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/) created when converting from a [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) type. See [connect_type_conv](#connect_type_conv).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-conv-size=#</code>
- <strong>Scope:</strong> Global, Session (&gt; [MariaDB 10.0.17](/kb/en/mariadb-10017-release-notes/)), Global (&lt;= [MariaDB 10.0.16](/kb/en/mariadb-10016-release-notes/))
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>
<ul start="1"><li>&gt;= [MariaDB 10.4.8](/kb/en/mariadb-1048-release-notes/), [MariaDB 10.3.18](/kb/en/mariadb-10318-release-notes/), [MariaDB 10.2.27](/kb/en/mariadb-10227-release-notes/): `1024`
</li><li>&lt;= [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/): `8192` 
</li></ul>
- <strong>Range:</strong> `0` to `65500`

---

#### `connect_default_depth`

- <strong>Description:</strong> Default depth used by Json, XML and Mongo discovery.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-default-depth=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>`5`
- <strong>Range:</strong> `-1` to `16`
- <strong>Introduced:</strong> [MariaDB 10.5.7](/kb/en/mariadb-1057-release-notes/), [MariaDB 10.4.16](/kb/en/mariadb-10416-release-notes/), [MariaDB 10.3.26](/kb/en/mariadb-10326-release-notes/), [MariaDB 10.2.35](/kb/en/mariadb-10235-release-notes/)

---

#### `connect_default_prec`

- <strong>Description:</strong> Default precision used for doubles.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-default-prec=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>`6`
- <strong>Range:</strong> `0` to `16`
- <strong>Introduced:</strong> [MariaDB 10.5.9](/kb/en/mariadb-1059-release-notes/), [MariaDB 10.4.18](/kb/en/mariadb-10418-release-notes/), [MariaDB 10.3.28](/kb/en/mariadb-10328-release-notes/), [MariaDB 10.2.37](/kb/en/mariadb-10237-release-notes/)

---

#### `connect_enable_mongo`

- <strong>Description:</strong> Enable the [Mongo table type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-mongo-table-type/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-enable-mongo={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong>
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/), [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/), [MariaDB 10.1.28](/kb/en/mariadb-10128-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `connect_exact_info`

- <strong>Description:</strong> Whether the CONNECT engine should return an exact record number value to information queries. It is OFF by default because this information can take a very long time for large variable record length tables or for remote tables, especially if the remote server is not available. It can be set to ON when exact values are desired, for instance when querying the repartition of rows in a partition table.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-exact-info={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `connect_force_bson`

- <strong>Description:</strong> Force using BSON for JSON tables. Starting with these releases, the internal way JSON was parsed and handled was changed. The main advantage of the new way is to reduce the memory required to parse JSON (from 6 to 10 times the size of the JSON source to now only 2 to 4 times). However, this is in Beta mode and JSON tables are still handled using the old mode. To use the new mode, tables should be created with TABLE_TYPE=BSON, or by setting this session variable to 1 or ON. Then, all JSON tables will be handled as BSON. This is temporary until the new way replaces the old way by default.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-force-bson={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.5.9](/kb/en/mariadb-1059-release-notes/), [MariaDB 10.4.18](/kb/en/mariadb-10418-release-notes/), [MariaDB 10.3.28](/kb/en/mariadb-10328-release-notes/), [MariaDB 10.2.37](/kb/en/mariadb-10237-release-notes/)

---

#### `connect_indx_map`

- <strong>Description:</strong> Enable file mapping for index files. To accelerate the indexing process, CONNECT makes an index structure in memory from the index file. This can be done by reading the index file or using it as if it was in memory by “file mapping”. Set to 0 (file read, the default) or 1 (file mapping).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-indx-map=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `connect_java_wrapper`

- <strong>Description:</strong> Java wrapper.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-java-wrapper=val</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `wrappers/JdbcInterface`
- <strong>Introduced:</strong> Connect 1.05.0001, [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

---

#### `connect_json_all_path`

- <strong>Description:</strong> Discovery to generate json path for all columns if ON (the default) or do not when the path is the column name.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-json-all-path={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.5.7](/kb/en/mariadb-1057-release-notes/), [MariaDB 10.4.16](/kb/en/mariadb-10416-release-notes/), [MariaDB 10.3.26](/kb/en/mariadb-10326-release-notes/), [MariaDB 10.2.35](/kb/en/mariadb-10235-release-notes/)

---

#### `connect_json_grp_size`

- <strong>Description:</strong> Max number of rows for JSON aggregate functions.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-json-grp-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10`
- <strong>Range:</strong> `1` to `2147483647`
- <strong>Introduced:</strong> [MariaDB 10.0.17](/kb/en/mariadb-10017-release-notes/)

---

#### `connect_json_null`

- <strong>Description:</strong> Representation of JSON null values.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-json-null=value</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `&lt;null&gt;`
- <strong>Introduced:</strong> [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `connect_jvm_path`

- <strong>Description:</strong> Path to JVM library.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-jvm_path=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong>
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong>
- <strong>Introduced:</strong> Connect 1.04.0006

---

#### `connect_type_conv`

- <strong>Description:</strong> Determines the handling of [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) columns.
<ul start="1"><li>`NO`: The default until Connect 1.06.005, no conversion takes place, and a TYPE_ERROR is returned, resulting in a “not supported” message.
</li><li>`YES`: The default from Connect 1.06.006. The column is internally converted to a column declared as VARCHAR(n), `n` being the value of [connect_conv_size](#connect_conv_size).
</li><li>`FORCE` (&gt;= Connect 1.06.006): Also convert ODBC blob columns to TYPE_STRING.
</li><li>`SKIP`: No conversion. When the column declaration is provided via Discovery (meaning the CONNECT table is created without a column description), this column is not generated. Also applies to ODBC tables.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-type-conv=#</code>
- <strong>Scope:</strong> Global, Session (&gt; [MariaDB 10.0.17](/kb/en/mariadb-10017-release-notes/))
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Valid Values:</strong> `NO`, `YES` or `SKIP`
- <strong>Default Value:</strong>  `YES`

---

#### `connect_use_tempfile`

- <strong>Description:</strong> 
<ul><li>`NO`: The first algorithm is always used. Because it can cause errors when updating variable record length tables, this value should be set only for testing.
</li><li>`AUTO`: This is the default value. It leaves CONNECT to choose the algorithm to use. Currently it is equivalent to `NO`, except when updating variable record length tables ([DOS](/kb/en/connect-table-types-data-files/#dos-and-fix-table-types), [CSV](/kb/en/connect-table-types-data-files/#csv-and-fmt-table-types) or [FMT](/kb/en/connect-table-types-data-files/#fmt-type)) with file mapping forced to OFF.
</li><li>`YES`: Using a temporary file is chosen with some exceptions. These are when file mapping is ON, for [VEC](/kb/en/connect-table-types-data-files/#vec-table-type-vector) tables and when deleting from [DBF](/kb/en/connect-table-types-data-files/#dbf-type) tables (soft delete). For variable record length tables, file mapping is forced to OFF.
</li><li>`FORCE`: Like YES but forces file mapping to be OFF for all table types.
</li><li>`TEST`: Reserved for CONNECT development.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-use-tempfile=#</code>
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `AUTO`

---

#### `connect_work_size`

- <strong>Description:</strong> Size of the CONNECT work area used for memory allocation. Permits allocating a larger memory sub-allocation space when dealing with very large if sub-allocation fails. If the specified value is too big and memory allocation fails, the size of the work area remains but the variable value is not modified and should be reset.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-work-size=#</code>
- <strong>Scope:</strong> Global, Session (Session-only from CONNECT 1.03.005)
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `67108864`
- <strong>Range:</strong> `4194304` upwards, depending on the physical memory size

---

#### `connect_xtrace`

- <strong>Description:</strong> Console trace value. Set to `0` (no trace), or to other values if a console tracing is desired. Note that to test this handler, MariaDB should be executed with the [--console](/kb/en/mysqld-options/#-console) parameter because CONNECT prints some error and trace messages on the console. In some Linux versions, this is re-routed into the error log file. Console tracing can be set on the command line or later by names or values. Valid values (from Connect 1.06.006) include:
<ul start="1"><li>`0`: No trace
</li><li>`YES` or `1`: Basic trace
</li><li>`MORE` or `2`: More tracing
</li><li>`INDEX` or `4`: Index construction
</li><li>`MEMORY` or `8`: Allocating and freeing memory
</li><li>`SUBALLOC` or `16`: Sub-allocating in work area
</li><li>`QUERY` or `32`: Constructed query sent to external server
</li><li>`STMT` or `64`: Currently executing statement
</li><li>`HANDLER` or `128`: Creating and dropping CONNECT handlers 
</li><li>`BLOCK` or `256`: Creating and dropping CONNECT objects
</li><li>`MONGO` or `512`: Mongo and REST (from [Connect 1.06.0010](/columns-storage-engines-and-plugins/storage-engines/connect/)) tracing
</li></ul>
- For example: 
<ul><li>`set global connect_xtrace=0;                <em> No trace</em>`
</li><li>`set global connect_xtrace='YES';            <em> By name</em>`
</li><li>`set global connect_xtrace=1;                <em> By value</em>`
</li><li>`set global connect_xtrace='QUERY,STMT';     <em> By name</em>`
</li><li>`set global connect_xtrace=96;               <em> By value</em>`
</li><li>`set global connect_xtrace=1023;             <em> Trace all</em>`
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-xtrace=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `set`
- <strong>Default Value:</strong> `0`
- <strong>Valid Values:</strong> See description

---