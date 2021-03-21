# Setting the Language for Error Messages

MariaDB server error messages are by default in English. However, MariaDB server also supports error message localization in many different languages. Each supported language has its own version of the [error message file](/kb/en/error-log/#error-messages-file) called `errmsg.sys` in a dedicated directory for that language.

## Supported Languages for Error Messages

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, error message localization is supported for the following languages:

- Czech
- Danish
- Dutch
- English
- Estonian
- French
- German
- Greek
- Hindi
- Hungarian
- Italian
- Japanese
- Korean
- Norwegian
- Norwegian-ny (Nynorsk)
- Polish
- Portuguese
- Romanian
- Russian
- Serbian
- Slovak
- Spanish
- Swedish
- Ukrainian

##### MariaDB until [10.1](/kb/en/what-is-mariadb-101/)

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, error message localization is supported for the following languages:

- Czech
- Danish
- Dutch
- English
- Estonian
- French
- German
- Greek
- Hungarian
- Italian
- Japanese
- Korean
- Norwegian
- Norwegian-ny (Nynorsk)
- Polish
- Portuguese
- Romanian
- Russian
- Serbian
- Slovak
- Spanish
- Swedish
- Ukrainian

## Setting the `lc_messages` and `lc_messages_dir` System Variables

The <a undefined>lc_messages</a> and <a undefined>lc_messages_dir</a> system variables can be used to set the [server locale](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale) used for error messages.

The <a undefined>lc_messages</a> system variable can be specified as a [locale](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale) name. The language of the associated [locale](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale) will be used for error messages. See [Server Locales](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale) for a list of supported locales and their associated languages.

The <a undefined>lc_messages</a> system variable is set to `en_US` by default, which means that error messages are in English by default.

If the <a undefined>lc_messages</a> system variable is set to a valid [locale](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale) name, but the server can't find an [error message file](/kb/en/error-log/#error-messages-file) for the language associated with the [locale](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale), then the default language will be used instead.

This system variable can be specified as command-line arguments to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mariadb]
...
lc_messages=fr_CA
```

The <a undefined>lc_messages</a> system variable can also be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL lc_messages='fr_CA';
```

If a server has the <a undefined>lc_messages</a> system variable set to the `fr_CA` locale like the above example, then error messages would be in French. For example:

```sql
SELECT blah;
ERROR 1054 (42S22): Champ 'blah' inconnu dans field list
```

The <a undefined>lc_messages_dir</a> system variable can be specified either as the path to the directory storing the server's [error message files](/kb/en/error-log/#error-messages-file) or as the path to the directory storing the specific language's [error message file](/kb/en/error-log/#error-messages-file).

The server initially tries to interpret the value of the <a undefined>lc_messages_dir</a> system variable as a path to the directory storing the server's [error message files](/kb/en/error-log/#error-messages-file). Therefore, it constructs the path to the language's [error message file](/kb/en/error-log/#error-messages-file) by concatenating the value of the <a undefined>lc_messages_dir</a> system variable with the language name of the [locale](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale) specified by the <a undefined>lc_messages</a> system variable .

If the server does not find the [error message file](/kb/en/error-log/#error-messages-file) for the language, then it tries to interpret the value of the <a undefined>lc_messages_dir</a> system variable as a direct path to the directory storing the specific language's [error message file](/kb/en/error-log/#error-messages-file).

This system variable can be specified as command-line arguments to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files).

For example, to specify the path to the directory storing the server's [error message files](/kb/en/error-log/#error-messages-file):

```sql
[mariadb]
...
lc_messages_dir=/usr/share/mysql/
```

Or to specify the path to the directory storing the specific language's [error message file](/kb/en/error-log/#error-messages-file):

```sql
[mariadb]
...
lc_messages_dir=/usr/share/mysql/french/
```

The <a undefined>lc_messages_dir</a> system variable can not be changed dynamically.

## Setting the --language Option

The <a undefined>--language</a> option can also be used to set the server's language for error messages, but it is deprecated. It is recommended to set the <a undefined>lc_messages</a> system variable instead.

The <a undefined>--language</a> option can be specified either as a language name or as the path to the directory storing the language's [error message file](/kb/en/error-log/#error-messages-file). See [Server Locales](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale) for a list of supported locales and their associated languages.

This option can be specified as command-line arguments to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files).

For example, to specify a language name:

```sql
[mariadb]
...
language=french
```

Or to specify the path to the directory storing the language's [error message file](/kb/en/error-log/#error-messages-file):

```sql
[mariadb]
...
language=/usr/share/mysql/french/
```

## Character Set

The character set that the error messages are returned in is determined by the <a undefined>character_set_results</a> variable, which defaults to UTF8.