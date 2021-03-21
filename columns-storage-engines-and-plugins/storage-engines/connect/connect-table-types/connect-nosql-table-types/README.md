# CONNECT - NoSQL Table Types

They are based on files that do not match the relational format but often represent hierarchical data. CONNECT can handle [JSON](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-json-table-type/), [INI-CFG](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-ini-table-type/), [XML](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-xml-table-type/), and some HTML files.

The way it is done is different from what MySQL or PostgreSQL does. In addition to including in a table some column values of a specific data format (JSON, XML) to be handled by specific functions, CONNECT can directly use JSON, XML or INI files that are produced by other applications, and this is the table definition that describes where and how the contained information must be retrieved.

This is also different from what MariaDB does with dynamic columns, which is close to what MySQL and PostgreSQL do with the JSON column type.

Note: The LEVEL option used with these tables should, from Connect 1.07.0002, be specified as DEPTH. Also, what was specified with the FIELD_FORMAT column option should now also be specified using JPATH or XPATH.