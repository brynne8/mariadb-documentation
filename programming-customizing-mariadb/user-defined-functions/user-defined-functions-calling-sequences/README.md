# User-Defined Functions Calling Sequences

The functions described in [Creating User-defined Functions](/programming-customizing-mariadb/user-defined-functions/creating-user-defined-functions) are expanded on this page. They are declared as follows:

## Simple Functions

### x()

If x() returns an integer, it is declared as follows:

```sql
long long x(UDF_INIT *initid, UDF_ARGS *args,
              char *is_null, char *error);
```

If x() returns a string (DECIMAL functions also return string values), it is declared as follows:

```sql
char *x(UDF_INIT *initid, UDF_ARGS *args,
          char *result, unsigned long *length,
          char *is_null, char *error);
```

If x() returns a real, it is declared as follows:

```sql
double x(UDF_INIT *initid, UDF_ARGS *args,
              char *is_null, char *error);
```

### x_init()

```sql
my_bool x_init(UDF_INIT *initid, UDF_ARGS *args, char *message);
```

### x_deinit()

```sql
void x_deinit(UDF_INIT *initid);
```

### Description

<em>initid</em> is a parameter passed to all three functions that points to a <em>UDF_INIT</em> structure, used for communicating information between the functions. Its structure members are:

- my_bool maybe_null
<ul start="1"><li><em>maybe_null</em> should be set to <em>1</em> if x_init can return a NULL value, Defaults to <em>1</em> if any arguments are declared <em>maybe_null</em>.
</li></ul>
- unsigned int decimals
<ul start="1"><li>Number of decimals after the decimal point. The default, if an explicit number of decimals is passed in the arguments to the main function, is the maximum number of decimals, so if <em>9.5</em>, <em>9.55</em> and <em>9.555</em> are passed to the function, the default would be three (based on <em>9.555</em>, the maximum).  If there are no explicit number of decimals, the default is set to 31, or one more than the maximum for the DOUBLE, FLOAT and DECIMAL types. This default can be changed in the function to suit the actual calculation.
</li></ul>
- unsigned int max_length
<ul start="1"><li>Maximum length of the result. For integers, the default is 21. For strings, the length of the longest argument. For reals, the default is 13 plus the number of decimals indicated by <em>initid-&gt;decimals</em>. The length includes any signs or decimal points. Can also be set to 65KB or 16MB in order to return a BLOB. The memory remains unallocated, but this is used to decide on the data type to use if the data needs to be temporarily stored.
</li></ul>
- char *ptr
<ul start="1"><li>A pointer for use as required by the function. Commonly, <em>initid-&gt;ptr</em> is used to communicate allocated memory, with <em>x_init()</em> allocating the memory and assigning it to this pointer, <em>x()</em> using it, and <em>x_deinit()</em> de-allocating it.
</li></ul>
- my_bool const_item
<ul start="1"><li>Should be set to <em>1</em> in <em>x_init()</em> if <em>x()</em> always returns the same value, otherwise <em>0</em>.
</li></ul>

## Aggregate Functions

### x_clear()

<em>x_clear()</em> is a required function for aggregate functions, and is declared as follows:

```sql
void x_clear(UDF_INIT *initid, char *is_null, char *error);
```

It is called when the summary results need to be reset, that is at the beginning of each new group. but also to reset the values when there were no matching rows.

<em>is_null</em> is set to point to CHAR(0) before calling x_clear().

In the case of an error, you can store the value to which the error argument points (a single-byte variable, not a string string buffer) in the variable.

### x_reset()

<em>x_reset()</em> is declared as follows:

```sql
void x_reset(UDF_INIT *initid, UDF_ARGS *args,
               char *is_null, char *error);
```

It is called on finding the first row in a new group. Should reset the summary variables, and then use <em>UDF_ARGS</em> as the first value in the group's internal summary value. The function is not required if the UDF interface uses <em>x_clear()</em>.

### x_add()

<em>x_add()</em> is declared as follows:

```sql
void x_add(UDF_INIT *initid, UDF_ARGS *args,
             char *is_null, char *error);
```

It is called for all rows belonging to the same group, and should be used to add the value in <em>UDF_ARGS</em> to the internal summary variable.

### x_remove()

<em>x_remove()</em> was added in [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and is declared as follows (same as <em>x_add()</em>):

```sql
void x_remove(UDF_INIT* initid, UDF_ARGS* args,
               char* is_null, char *error );
```

It adds more efficient support of aggregate UDFs as [window functions](/built-in-functions/special-functions/window-functions). <em>x_remove()</em> should "subtract" the row (reverse <em>x_add()</em>). In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) aggregate UDFs will work as WINDOW functions without <em>x_remove()</em> but it will not be so efficient.

If <em>x_remove()</em> supported (defined) detected automatically.