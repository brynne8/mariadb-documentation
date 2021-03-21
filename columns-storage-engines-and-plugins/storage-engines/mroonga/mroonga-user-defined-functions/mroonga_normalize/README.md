# mroonga_normalize

## Syntax

```sql
mroonga_normalize(string[, normalizer_name])
```

## Description

`mroonga_normalize` is a [user-defined function](/programming-customizing-mariadb/user-defined-functions/) (UDF) included with the [Mroonga storage engine](/columns-storage-engines-and-plugins/storage-engines/mroonga/). It uses Groonga's normalizer to normalize text. See [Creating Mroonga User-Defined Functions](/columns-storage-engines-and-plugins/storage-engines/mroonga/mroonga-user-defined-functions/creating-mroonga-user-defined-functions/) for details on creating this UDF if required.

Given a string, returns the normalized text.

See the [Groonga Normalizer Reference](http://groonga.org/docs/reference/normalizers.html) for details on the Groonga normalizers. The default if no normalizer is provided is `NormalizerAuto`.

## Examples

```sql
SELECT mroonga_normalize("ABぃ㍑");
+-------------------------------+
| mroonga_normalize("ABぃ㍑")   |
+-------------------------------+
| abぃリットル                  |
+-------------------------------+
```

## See Also

- [Creating Mroonga User-Defined Functions](/columns-storage-engines-and-plugins/storage-engines/mroonga/mroonga-user-defined-functions/creating-mroonga-user-defined-functions/)