# Table Discovery (before 10.0.2)

This page describes the <strong>old</strong> discovery API, created in MySQL for NDB Cluster. It no longer works in MariaDB. New table discovery API is documented [here](/columns-storage-engines-and-plugins/storage-engines/storage-engines-storage-engine-development/table-discovery/).

There are four parts of it.

First, when a server finds that a table (for example, mentioned in the `SELECT` query) does not exist, it asks every engine whether it knows anything about this table. For this it uses <strong>discover()</strong> method of the handlerton. The method is defined as

```sql
int discover(handlerton *hton, THD* thd, const char *db, const char *name,
             unsigned char **frmblob, size_t *frmlen);
```

It takes the database and a table name as arguments and is returns 0 if the table exists in the engine and 1 otherwise. If it returned 0, it is supposed to allocate (with `my_malloc()`) a buffer and store the complete binary image of the `.frm` file of that table. The server will write it down to disk, creating table's `.frm`. The output parameters `frmblob` and `frmlen` are used to return the information about the buffer to the caller. The caller is responsible for freeing the buffer with `my_free()`.

Second, in some cases the server only wants to know if the table exists, but it does not really need to open it and will not use its `.frm` file image. Then using the `discover()` method would be an overkill, and the server uses a lightweight <strong>table_exists_in_engine()</strong> method. This method is defined as

```sql
int table_exists_in_engine(handlerton *hton, THD* thd,
                           const char *db, const char *name);
```

and it returns one of the `HA_ERR_` codes, typically `HA_ERR_NO_SUCH_TABLE` or <code>
HA_ERR_TABLE_EXIST</code>.

Third, there can be a situation when the server thinks that the table exists (it found and successfully read the `.frm` file), but from the engine point of view the `.frm` file is incorrect. For example, the table was already deleted from the engine, or its definition was modified (again, modified only in the engine). In this case the `.frm` file is outdated, and the server needs to re-discover the table. The engine conveys this to the server by returning `HA_ERR_TABLE_DEF_CHANGED` error code from the handler's <strong>open()</strong> method. On receiving this error the server will use the `discover()` method to get the new `.frm` image. This also means that <em>after</em> the table is opened, the server does not expect its metadata to change. The engine thus should ensure (with some kind of locking, perhaps) that a table metadata cannot be modified, as long as the table stays opened.

And fourth, a user might want to retrieve a list of tables in a specific database. With `SHOW TABLES` or by quering `INFORMATION_SCHEMA` tables. The user expects to see all tables, but the server cannot discover them one by one, because it doesnt know table names. In this case, the server uses a special discovery technique. It is <strong>find_files()</strong> method in the handlerton, defines as

```sql
int find_files(handlerton *hton, THD *thd,
               const char *db, const char *path,
               const char *wild, bool dir, List<LEX_STRING> *files);
```

and  it, typically for Storage Engine API, returns 0 on success and 1 on failure. The arguments mean `db` - the name of the database, `path` - the path to it, `wild` an SQL wildcard pattern (for example, from `SHOW TABLES LIKE '...'`, and `dir`, if set, means to discover <em>databases</em> instead of <em>tables</em>.