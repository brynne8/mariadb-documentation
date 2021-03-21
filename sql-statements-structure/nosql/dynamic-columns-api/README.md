# Dynamic Columns API

This page describes client-side of [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/) API and MariaDB Connector/C 2.0 for reading and writing [Dynamic Columns](/sql-statements-structure/nosql/dynamic-columns/) blobs.

Normally, you should use [Dynamic column functions](/kb/en/dynamic-columns/#dynamic-columns-functions) which are run inside the MariaDB server and allow one to access Dynamic Columns content without any client-side libraries.

If you need to read/write dynamic column blobs <strong>on the client</strong> for some reason, this API enables that.

## Where to get it

The API is a part of `libmysql` C client library.  In order to use it, one needs to include this header file

```sql
#include <mysql/ma_dyncol.h>
```

and link against `libmysql`.

## Data structures

### DYNAMIC_COLUMN

`DYNAMIC_COLUMN` represents a packed dynamic column blob.  It is essentially a string-with-length and is defined as follows:

```sql
/* A generic-purpose arbitrary-length string defined in MySQL Client API */
typedef struct st_dynamic_string
{
  char *str;
  size_t length,max_length,alloc_increment;
} DYNAMIC_STRING;

...

typedef DYNAMIC_STRING DYNAMIC_COLUMN;
```

### DYNAMIC_COLUMN_VALUE

Dynamic columns blob stores {name, value} pairs. `DYNAMIC_COLUMN_VALUE` structure is used to represent the value in accessible form.

```sql
struct st_dynamic_column_value
{
  DYNAMIC_COLUMN_TYPE type;
  union
  {
    long long long_value;
    unsigned long long ulong_value;
    double double_value;
    struct {
      MYSQL_LEX_STRING value;
      CHARSET_INFO *charset;
    } string;
    struct {
      decimal_digit_t buffer[DECIMAL_BUFF_LENGTH];
      decimal_t value;
    } decimal;
    MYSQL_TIME time_value;
  } x;
};
typedef struct st_dynamic_column_value DYNAMIC_COLUMN_VALUE;
```

Every value has a type, which is determined by the `type` member.

<table><tbody><tr><th>type</th><th>structure field</th></tr>
<tr><td><code>DYN_COL_NULL</code></td><td>-</td></tr>
<tr><td><code>DYN_COL_INT</code></td><td><code>value.x.long_value</code></td></tr>
<tr><td><code>DYN_COL_UINT</code></td><td><code>value.x.ulong_value</code></td></tr>
<tr><td><code>DYN_COL_DOUBLE</code></td><td><code>value.x.double_value</code></td></tr>
<tr><td><code>DYN_COL_STRING</code></td><td><code>value.x.string.value</code>, <code>value.x.string.charset</code></td></tr>
<tr><td><code>DYN_COL_DECIMAL</code></td><td><code>value.x.decimal.value</code></td></tr>
<tr><td><code>DYN_COL_DATETIME</code></td><td><code>value.x.time_value</code></td></tr>
<tr><td><code>DYN_COL_DATE</code></td><td><code>value.x.time_value</code></td></tr>
<tr><td><code>DYN_COL_TIME</code></td><td><code>value.x.time_value</code></td></tr>
<tr><td><code>DYN_COL_DYNCOL</code></td><td><code>value.x.string.value</code></td></tr>
</tbody></table>

Notes

- Values with type `DYN_COL_NULL` do not ever occur in dynamic columns blobs.
- Type `DYN_COL_DYNCOL` means that the value is a packed dynamic blob. This is how nested dynamic columns are done.
- Before storing a value to `value.x.decimal.value`, one must call `mariadb_dyncol_prepare_decimal()` to initialize the space for storage.

### enum_dyncol_func_result

`enum enum_dyncol_func_result` is used as return value.

<table><tbody><tr><th>value</th><th>name</th><th>meaning</th></tr>
<tr><td>0</td><td><code>ER_DYNCOL_OK</code></td><td>OK</td></tr>
<tr><td>0</td><td><code>ER_DYNCOL_NO</code></td><td>(the same as ER_DYNCOL_OK but for functions which return a YES/NO)</td></tr>
<tr><td>1</td><td><code>ER_DYNCOL_YES</code></td><td>YES response or success</td></tr>
<tr><td>2</td><td><code>ER_DYNCOL_TRUNCATED</code></td><td>Operation succeeded but the data was truncated</td></tr>
<tr><td>-1</td><td><code>ER_DYNCOL_FORMAT</code></td><td>Wrong format of the encoded string</td></tr>
<tr><td>-2</td><td><code>ER_DYNCOL_LIMIT</code></td><td>A limit of implementation reached</td></tr>
<tr><td>-3</td><td><code>ER_DYNCOL_RESOURCE</code></td><td>Out of resources</td></tr>
<tr><td>-4</td><td><code>ER_DYNCOL_DATA</code></td><td>Incorrect input data</td></tr>
<tr><td>-5</td><td><code>ER_DYNCOL_UNKNOWN_CHARSET</code></td><td>Unknown character set</td></tr>
</tbody></table>

Result codes that are less than zero represent error conditions.

## Function reference

Functions come in pairs:

- `xxx_num()` operates on the old (pre-MariaDB-10.0.1) dynamic column blob format where columns were identified by numbers.
- `xxx_named()` can operate on both old or new data format. If it modifies the blob, it will convert it to the new data format.

You should use `xxx_named()` functions, unless you need to keep the data compatible with MariaDB versions before 10.0.1.

### mariadb_dyncol_init

1 define mariadb_dyncol_init(A) memset((A), 0, sizeof(*(A)))

It is correct initialization for an empty packed dynamic blob.

### mariadb_dyncol_free

```sql
void mariadb_dyncol_free(DYNAMIC_COLUMN *str);
```

where

<table><tbody><tr><td><code>str</code></td><td><code>IN</code></td><td>Packed dynamic blob which memory should be freed.</td></tr>
</tbody></table>

### mariadb_dyncol_create_many (num|named)

Create a packed dynamic blob from arrays of values and names.

```sql
enum enum_dyncol_func_result
mariadb_dyncol_create_many_num(DYNAMIC_COLUMN *str,
                               uint column_count,
                               uint *column_numbers,
                               DYNAMIC_COLUMN_VALUE *values,
                               my_bool new_string);
enum enum_dyncol_func_result
mariadb_dyncol_create_many_named(DYNAMIC_COLUMN *str,
                                 uint column_count,
                                 MYSQL_LEX_STRING *column_keys,
                                 DYNAMIC_COLUMN_VALUE *values,
                                 my_bool new_string);
```

where

<table><tbody><tr><td><code>str</code></td><td><code>OUT</code></td><td>Packed dynamic blob will be put here</td></tr>
<tr><td><code>column_count</code></td><td><code>IN</code></td><td>Number of columns</td></tr>
<tr><td><code>column_numbers</code></td><td><code>IN</code></td><td>Column numbers array (old format)</td></tr>
<tr><td><code>column_keys</code></td><td><code>IN</code></td><td>Column names array (new format)</td></tr>
<tr><td><code>values</code></td><td><code>IN</code></td><td>Column values array</td></tr>
<tr><td><code>new_string</code></td><td><code>IN</code></td><td>If TRUE then the <code>str</code> will be reinitialized (not freed) before usage</td></tr>
</tbody></table>

### mariadb_dyncol_update_many (num|named)

Add or update columns in a dynamic columns blob.  To delete a column, update its value to a "non-value" of type `DYN_COL_NULL`

```sql
enum enum_dyncol_func_result
mariadb_dyncol_update_many_num(DYNAMIC_COLUMN *str,
                               uint column_count,
                               uint *column_numbers,
                               DYNAMIC_COLUMN_VALUE *values);
enum enum_dyncol_func_result
mariadb_dyncol_update_many_named(DYNAMIC_COLUMN *str,
                                 uint column_count,
                                 MYSQL_LEX_STRING *column_keys,
                                 DYNAMIC_COLUMN_VALUE *values);
```

<table><tbody><tr><td><code>str</code></td><td><code>IN/OUT</code></td><td>Dynamic columns blob to be modified.</td></tr>
<tr><td><code>column_count</code></td><td><code>IN</code></td><td>Number of columns in following arrays</td></tr>
<tr><td><code>column_numbers</code></td><td><code>IN</code></td><td>Column numbers array (old format)</td></tr>
<tr><td><code>column_keys</code></td><td><code>IN</code></td><td>Column names array (new format)</td></tr>
<tr><td><code>values</code></td><td><code>IN</code></td><td>Column values array</td></tr>
</tbody></table>

### mariadb_dyncol_exists (num|named)

Check if column with given name exists in the blob

```sql
enum enum_dyncol_func_result
mariadb_dyncol_exists_num(DYNAMIC_COLUMN *str, uint column_number);
enum enum_dyncol_func_result
mariadb_dyncol_exists_named(DYNAMIC_COLUMN *str, MYSQL_LEX_STRING *column_key);
```

<table><tbody><tr><td><code>str</code></td><td><code>IN</code></td><td>Packed dynamic columns string.</td></tr>
<tr><td><code>column_number</code></td><td><code>IN</code></td><td>Column number (old format)</td></tr>
<tr><td><code>column_key</code></td><td><code>IN</code></td><td>Column name (new format)</td></tr>
</tbody></table>

The function returns YES/NO or Error code

### mariadb_dyncol_column_count

Get number of columns in a dynamic column blob

```sql
enum enum_dyncol_func_result
mariadb_dyncol_column_count(DYNAMIC_COLUMN *str, uint *column_count);
```

<table><tbody><tr><td><code>str</code></td><td><code>IN</code></td><td>Packed dynamic columns string.</td></tr>
<tr><td><code>column_count</code></td><td><code>OUT</code></td><td>Number of not NULL columns in the dynamic columns string</td></tr>
</tbody></table>

### mariadb_dyncol_list (num|named)

List columns in a dynamic column blob.

```sql
enum enum_dyncol_func_result
mariadb_dyncol_list_num(DYNAMIC_COLUMN *str, uint *column_count, uint **column_numbers);
enum enum_dyncol_func_result
mariadb_dyncol_list_named(DYNAMIC_COLUMN *str, uint *column_count,
                          MYSQL_LEX_STRING **column_keys);
```

<table><tbody><tr><td><code>str</code></td><td><code>IN</code></td><td>Packed dynamic columns string.</td></tr>
<tr><td><code>column_count</code></td><td><code>OUT</code></td><td>Number of columns in following arrays</td></tr>
<tr><td><code>column_numbers</code></td><td><code>OUT</code></td><td>Column numbers array (old format). Caller should free this array.</td></tr>
<tr><td><code>column_keys</code></td><td><code>OUT</code></td><td>Column names array (new format). Caller should free this array.</td></tr>
</tbody></table>

### mariadb_dyncol_get (num|named)

Get a value of one column

```sql
enum enum_dyncol_func_result
mariadb_dyncol_get_num(DYNAMIC_COLUMN *org, uint column_number,
                       DYNAMIC_COLUMN_VALUE *value);
enum enum_dyncol_func_result
mariadb_dyncol_get_named(DYNAMIC_COLUMN *str, MYSQL_LEX_STRING *column_key,
                         DYNAMIC_COLUMN_VALUE *value);
```

<table><tbody><tr><td><code>str</code></td><td><code>IN</code></td><td>Packed dynamic columns string.</td></tr>
<tr><td><code>column_number</code></td><td><code>IN</code></td><td>Column numbers array (old format)</td></tr>
<tr><td><code>column_key</code></td><td><code>IN</code></td><td>Column names array (new format)</td></tr>
<tr><td><code>value</code></td><td><code>OUT</code></td><td>Value of the column</td></tr>
</tbody></table>

If the column is not found NULL returned as a value of the column.

### mariadb_dyncol_unpack

Get value of all columns

```sql
enum enum_dyncol_func_result
mariadb_dyncol_unpack(DYNAMIC_COLUMN *str,
                      uint *column_count,
                      MYSQL_LEX_STRING **column_keys,
                      DYNAMIC_COLUMN_VALUE **values);
```

<table><tbody><tr><td><code>str</code></td><td><code>IN</code></td><td>Packed dynamic columns string to unpack.</td></tr>
<tr><td><code>column_count</code></td><td><code>OUT</code></td><td>Number of columns in following arrays</td></tr>
<tr><td><code>column_keys</code></td><td><code>OUT</code></td><td>Column names array (should be free by caller)</td></tr>
<tr><td><code>values</code></td><td><code>OUT</code></td><td>Values of the columns array (should be free by caller)</td></tr>
</tbody></table>

### mariadb_dyncol_has_names

Check whether the dynamic columns blob uses new data format (the one where columns are identified by names)

```sql
my_bool mariadb_dyncol_has_names(DYNAMIC_COLUMN *str);
```

<table><tbody><tr><td><code>str</code></td><td><code>IN</code></td><td>Packed dynamic columns string.</td></tr>
</tbody></table>

### mariadb_dyncol_check

Check whether dynamic column blob has correct data format.

```sql
enum enum_dyncol_func_result
mariadb_dyncol_check(DYNAMIC_COLUMN *str);
```

<table><tbody><tr><td><code>str</code></td><td><code>IN</code></td><td>Packed dynamic columns string.</td></tr>
</tbody></table>

### mariadb_dyncol_json

Get contents od a dynamic columns blob in a JSON form

```sql
enum enum_dyncol_func_result
mariadb_dyncol_json(DYNAMIC_COLUMN *str, DYNAMIC_STRING *json);
```

<table><tbody><tr><td><code>str</code></td><td><code>IN</code></td><td>Packed dynamic columns string.</td></tr>
<tr><td><code>json</code></td><td><code>OUT</code></td><td>JSON representation</td></tr>
</tbody></table>

mariadb_dyncol_json() allocates memory for the parameter json which must be explicitly freed by the mariadb_dyncol_free() function to prevent memory leakage.

### mariadb_dyncol_val_TYPE

Get dynamic column value as one of the base types

```sql
enum enum_dyncol_func_result
mariadb_dyncol_val_str(DYNAMIC_STRING *str, DYNAMIC_COLUMN_VALUE *val,
                       CHARSET_INFO *cs, my_bool quote);
enum enum_dyncol_func_result
mariadb_dyncol_val_long(longlong *ll, DYNAMIC_COLUMN_VALUE *val);
enum enum_dyncol_func_result
mariadb_dyncol_val_double(double *dbl, DYNAMIC_COLUMN_VALUE *val);
```

<table><tbody><tr><td><code>str</code> or <code>ll</code> or <code>dbl</code></td><td><code>OUT</code></td><td>value of the column</td></tr>
<tr><td><code>val</code></td><td><code>IN</code></td><td>Value</td></tr>
</tbody></table>

### mariadb_dyncol_prepare_decimal

Initialize `DYNAMIC_COLUMN_VALUE` before value of `value.x.decimal.value` can be set

```sql
void mariadb_dyncol_prepare_decimal(DYNAMIC_COLUMN_VALUE *value);
```

<table><tbody><tr><td><code>value</code></td><td><code>OUT</code></td><td>Value of the column</td></tr>
</tbody></table>

This function links `value.x.decimal.value` to `value.x.decimal.buffer`.

### mariadb_dyncol_value_init

Initialize a `DYNAMIC_COLUMN_VALUE` structure to a safe default.

```sql
#define mariadb_dyncol_value_init(V) (V)->type= DYN_COL_NULL
```

### mariadb_dyncol_column_cmp_named

Compare two column names for equality

```sql
int mariadb_dyncol_column_cmp_named(const MYSQL_LEX_STRING *s1,
                                    const MYSQL_LEX_STRING *s2);
```