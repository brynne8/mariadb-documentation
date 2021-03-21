# SHOW LOCALES

##### MariaDB starting with [10.1.1](/kb/en/mariadb-1011-release-notes/)

`SHOW LOCALES` was introduced as part of the [Information Schema plugin extension](/kb/en/information-schema-plugins-show-and-flush-statements/) in [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/).

`SHOW LOCALES` is used to return [locales](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale) information as part of the [Locales](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/locales-plugin) plugin. While the <a undefined>information_schema.LOCALES</a> table has 8 columns, the `SHOW LOCALES` statement will only display 4 of them:

## Example

```sql
SHOW LOCALES;
+-----+-------+-------------------------------------+------------------------+
| Id  | Name  | Description                         | Error_Message_Language |
+-----+-------+-------------------------------------+------------------------+
|   0 | en_US | English - United States             | english                |
|   1 | en_GB | English - United Kingdom            | english                |
|   2 | ja_JP | Japanese - Japan                    | japanese               |
|   3 | sv_SE | Swedish - Sweden                    | swedish                |
...
```