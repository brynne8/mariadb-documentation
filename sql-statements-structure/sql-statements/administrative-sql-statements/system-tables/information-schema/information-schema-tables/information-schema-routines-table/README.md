# Information Schema ROUTINES Table

The [Information Schema](/kb/en/information_schema/) `ROUTINES` table stores information about [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures/) and [stored functions](/programming-customizing-mariadb/stored-routines/stored-functions/).

It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>SPECIFIC_NAME</code></td><td></td></tr>
<tr><td><code>ROUTINE_CATALOG</code></td><td>Always <code>def</code>.</td></tr>
<tr><td><code>ROUTINE_SCHEMA</code></td><td>Database name associated with the routine.</td></tr>
<tr><td><code>ROUTINE_NAME</code></td><td>Name of the routine.</td></tr>
<tr><td><code>ROUTINE_TYPE</code></td><td>Whether the routine is a <code>PROCEDURE</code> or a <code>FUNCTION</code>.</td></tr>
<tr><td><code>DATA_TYPE</code></td><td>The return value's <a href="/kb/en/data-types/">data type</a> (for stored functions).</td></tr>
<tr><td><code>CHARACTER_MAXIMUM_LENGTH</code></td><td>Maximum length.</td></tr>
<tr><td><code>CHARACTER_OCTET_LENGTH</code></td><td>Same as the <code>CHARACTER_MAXIMUM_LENGTH</code> except for multi-byte <a href="/kb/en/data-types-character-sets-and-collations/">character sets</a>.</td></tr>
<tr><td><code>NUMERIC_PRECISION</code></td><td>For numeric types, the precision (number of significant digits) for the column. <code>NULL</code> if not a numeric field.</td></tr>
<tr><td><code>NUMERIC_SCALE</code></td><td>For numeric types, the scale (significant digits to the right of the decimal point). NULL if not a numeric field.</td></tr>
<tr><td><code>DATETIME_PRECISION</code></td><td>Fractional-seconds precision, or NULL if not a <a href="/kb/en/date-and-time-data-types/">time data type</a>.</td></tr>
<tr><td><code>CHARACTER_SET_NAME</code></td><td><a href="/kb/en/data-types-character-sets-and-collations/">Character set</a> if a non-binary <a href="/kb/en/string-data-types/">string data type</a>, otherwise <code>NULL</code>.</td></tr>
<tr><td><code>COLLATION_NAME</code></td><td><a href="/kb/en/data-types-character-sets-and-collations/">Collation</a> if a non-binary <a href="/kb/en/string-data-types/">string data type</a>, otherwise NULL.</td></tr>
<tr><td><code>DATA_TYPE</code></td><td>The column's <a href="/kb/en/data-types/">data type</a>.</td></tr>
<tr><td><code>ROUTINE_BODY</code></td><td>Always <code>SQL</code>.</td></tr>
<tr><td><code>ROUTINE_DEFINITION</code></td><td>Definition of the routine.</td></tr>
<tr><td><code>EXTERNAL_NAME</code></td><td>Always <code>NULL</code>.</td></tr>
<tr><td><code>EXTERNAL_LANGUAGE</code></td><td>Always <code>SQL</code>.</td></tr>
<tr><td><code>PARAMETER_STYLE</code></td><td>Always <code>SQL</code>.</td></tr>
<tr><td><code>IS_DETERMINISTIC</code></td><td>Whether the routine is deterministic (can produce only one result for a given list of parameters) or not.</td></tr>
<tr><td><code>SQL_DATA_ACCESS</code></td><td>One of <code>READS SQL DATA</code>, <code>MODIFIES SQL DATA</code>, <code>CONTAINS SQL</code>, or <code>NO SQL</code>.</td></tr>
<tr><td><code>SQL_PATH</code></td><td>Always <code>NULL</code>.</td></tr>
<tr><td><code>SECURITY_TYPE</code></td><td><code>INVOKER</code> or <code>DEFINER</code>. Indicates which user's privileges apply to this routine.</td></tr>
<tr><td><code>CREATED</code></td><td>Date and time the routine was created.</td></tr>
<tr><td><code>LAST_ALTERED</code></td><td>Date and time the routine was last changed.</td></tr>
<tr><td><code>SQL_MODE</code></td><td>The <code><a href="/kb/en/sql-mode/">SQL_MODE</a></code> at the time the routine was created.</td></tr>
<tr><td><code>ROUTINE_COMMENT</code></td><td>Comment associated with the routine.</td></tr>
<tr><td><code>DEFINER</code></td><td>If the <code>SECURITY_TYPE</code> is <code>DEFINER</code>, this value indicates which user defined this routine.</td></tr>
<tr><td><code>CHARACTER_SET_CLIENT</code></td><td>The <a href="/kb/en/data-types-character-sets-and-collations/">character set</a> used by the client that created the routine.</td></tr>
<tr><td><code>COLLATION_CONNECTION</code></td><td>The <a href="/kb/en/data-types-character-sets-and-collations/">collation</a> (and character set) used by the connection that created the routine.</td></tr>
<tr><td><code>DATABASE_COLLATION</code></td><td>The default <a href="/kb/en/data-types-character-sets-and-collations/">collation</a> (and character set) for the database, at the time the routine was created.</td></tr>
</tbody></table>

It provides information similar to, but more complete, than the [SHOW PROCEDURE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-procedure-status/) and [SHOW FUNCTION STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-function-status/) statements.

For information about the parameters accepted by the routine, you can query the <a undefined>information_schema.PARAMETERS</a> table.

## See also

- [Stored Function Overview](/programming-customizing-mariadb/stored-routines/stored-functions/stored-function-overview/)
- [Stored Procedure Overview](/programming-customizing-mariadb/stored-routines/stored-procedures/stored-procedure-overview/)