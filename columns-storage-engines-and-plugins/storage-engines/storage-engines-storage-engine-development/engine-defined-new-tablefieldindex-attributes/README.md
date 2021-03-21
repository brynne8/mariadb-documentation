# Engine-defined New Table/Field/Index Attributes

In MariaDB, a storage engine can allow the user to specify additional attributes per index, field, or table. The engine needs to declare what attributes it introduces.

## API

There are three new members in the <code class="fixed" style="white-space:pre-wrap">handlerton</code> structure, they can be set in the engine's initialization function as follows:

```sql
example_hton->table_options= example_table_option_array;
example_hton->field_options= example_field_option_array;
example_hton->index_options= example_index_option_array;
```

The arrays are declared statically, as in the following example:

```sql
static MYSQL_THDVAR_ULONG(varopt_default, PLUGIN_VAR_RQCMDARG,
  "default value of the VAROPT table option", NULL, NULL, 5, 0, 100, 0);

struct ha_table_option_struct
{
  char *strparam;
  ulonglong ullparam;
  uint enumparam;
  bool boolparam;
  ulonglong varparam;
};

ha_create_table_option example_table_option_list[]=
{
  HA_TOPTION_NUMBER("NUMBER", ullparam, UINT_MAX32, 0, UINT_MAX32, 10),
  HA_TOPTION_STRING("STR", strparam),
  HA_TOPTION_ENUM("ONE_OR_TWO", enumparam, "one,two", 0),
  HA_TOPTION_BOOL("YESNO", boolparam, 1),
  HA_TOPTION_SYSVAR("VAROPT", varopt, varparam),
  HA_TOPTION_END
};
```

The engine declares a structure 
<code class="fixed" style="white-space:pre-wrap"><span class="n">ha_table_option_struct</span>
</code>
that will hold values of these new attributes.

And it describes these attributes to MySQL by creating an array of 
<code class="fixed" style="white-space:pre-wrap"><span class="n">HA_TOPTION_</span><span class="o">*</span>
</code> macros. Note a detail: these macros expect a structure called 
<code class="fixed" style="white-space:pre-wrap"><span class="n">ha_table_option_struct</span>
</code>, if the structure is called differently, a 
<code class="fixed" style="white-space:pre-wrap"><span class="cp">#define</span>
</code> will be needed.

There are five supported kinds of attributes:

<table><tbody><tr><th>macro name</th><th>attribure value type</th><th>corresponding<span>&nbsp;</span>C<span>&nbsp;</span>type</th><th>additional parameters of a macro</th></tr>
<tr><td><code>HA_TOPTION_NUMBER</code></td><td>an integer number</td><td><code>unsigned long long</code></td><td>a default value, minimal allowed value, maximal allowed value, a factor, that any allowed should be a multiple of.</td></tr>
<tr><td><code>HA_TOPTION_STRING</code></td><td>a string</td><td><code>char *</code></td><td>none. The default value is a null pointer.</td></tr>
<tr><td><code>HA_TOPTION_ENUM</code></td><td>one value from a list of allowed values</td><td><code>unsigned int</code></td><td>a string with a comma-separated list of allowed values, and a default value as a number, starting from 0.</td></tr>
<tr><td><code>HA_TOPTION_BOOL</code></td><td>a boolean</td><td><code>bool</code></td><td>a default value</td></tr>
<tr><td><code>HA_TOPTION_SYSVAR</code></td><td>defined by the system variable</td><td>defined by the system variable</td><td>system variable name</td></tr>
</tbody></table>

<em>Do <strong>not</strong> use </em> <code class="fixed" style="white-space:pre-wrap">enum</code> <em> for your </em> <code class="fixed" style="white-space:pre-wrap">HA_TOPTION_ENUM</code> <em> C structure members, the size of the </em> <code class="fixed" style="white-space:pre-wrap">enum</code> <em> depends on the compiler, and even on the compilation options, and the plugin API uses only types with known storage sizes.</em>

In all macros the first two parameters are name of the attribute as should be used in SQL in the <code class="fixed" style="white-space:pre-wrap">CREATE TABLE</code> statement, and the name of the corresponding member of the <code class="fixed" style="white-space:pre-wrap">ha_table_option_struct</code> structure.

The <code class="fixed" style="white-space:pre-wrap">HA_TOPTION_SYSVAR</code> stands aside a bit. It does not specify the attribute type or the default value, instead it binds the attribute to a system variable. The attribute type and the range of allowed values will be the same as of the corresponding system variable. The attribute <strong>default value</strong> will be the <strong>current value</strong> of its system variable. And unlike other attribute types that are only stored in the `.frm` file if explicitly set in the `CREATE TABLE` statement, the <code class="fixed" style="white-space:pre-wrap">HA_TOPTION_SYSVAR</code> attributes are always stored. If the system variable value is changed, it will not affect existing tables. Note that for this very reason, if a table was created in the old version of a storage engine, and a new version has introduced a <code class="fixed" style="white-space:pre-wrap">HA_TOPTION_SYSVAR</code> attribute, the attribute value in the old tables will be the <strong>default</strong> value of the system variable, not its <strong>current</strong> value.

The array ends with a <code class="fixed" style="white-space:pre-wrap">HA_TOPTION_END</code> macro.

Field and index (key) attributes are declared similarly using <code class="fixed" style="white-space:pre-wrap">HA_FOPTION_*</code> and <code class="fixed" style="white-space:pre-wrap">HA_IOPTION_*</code> macros.

When in a <code class="fixed" style="white-space:pre-wrap">CREATE TABLE</code> statement, the <code class="fixed" style="white-space:pre-wrap">::create()</code> handler method is called, the table attributes are available in the <code class="fixed" style="white-space:pre-wrap">table_arg-&gt;s-&gt;option_struct</code>, field attributes - in the <code class="fixed" style="white-space:pre-wrap">option_struct</code> member of the individual fields (objects of the <code class="fixed" style="white-space:pre-wrap">Field</code> class), index attributes - in the <code class="fixed" style="white-space:pre-wrap">option_struct</code> member of the individual keys (objects of the <code class="fixed" style="white-space:pre-wrap">KEY</code> class).

Additionally, they are available in most other handler methods: the attributes are stored in the <code class="fixed" style="white-space:pre-wrap">.frm</code> file and on every open MySQL makes them available to the engine by filling the corresponding <code class="fixed" style="white-space:pre-wrap">option_struct</code> members of the table, fields, and keys.

The <code class="fixed" style="white-space:pre-wrap">ALTER TABLE</code> needs a special support from the engine. MySQL compares old and new table definitions to decide whether it needs to rebuild the table or not. As the semantics of the engine declared attributes is unknown, MySQL cannot make this decision by analyzing attribute values - this is delegated to the engine. The <code class="fixed" style="white-space:pre-wrap">HA_CREATE_INFO</code> structure has three new members:

```sql
ha_table_option_struct *option_struct;           ///< structure with parsed table options
ha_field_option_struct **fields_option_struct;   ///< array of field option structures
ha_index_option_struct **indexes_option_struct;  ///< array of index option structures
```

The engine (in the <code class="fixed" style="white-space:pre-wrap">::check_if_incompatible_data()</code> method) is responsible for comparing new values of the attributes from the <code class="fixed" style="white-space:pre-wrap">HA_CREATE_INFO</code> structure with the old values from the table and returning <code class="fixed" style="white-space:pre-wrap">COMPATIBLE_DATA_NO</code> if they were changed in such a way that requires the table to be rebuild.

The example of declaring the attributes and comparing the values for the <code class="fixed" style="white-space:pre-wrap">ALTER TABLE</code> can be found in the EXAMPLE engine.

## SQL

The engine declared attributes can be specified per field, index, or table in the <code class="fixed" style="white-space:pre-wrap">CREATE TABLE</code> or <code class="fixed" style="white-space:pre-wrap">ALTER TABLE</code>. The syntax is the conventional:

```sql
CREATE TABLE ... (
  field ... [attribute=value [attribute=value ...]],
  ...
  index ... [attribute=value [attribute=value ...]],
  ...
) ...  [attribute=value [attribute=value ...]]
```

All values must be specified as literals, not expressions. The value of a boolean option may be specified as one of YES, NO, ON, OFF, 1, or 0. A string value may be specified either quoted or not, as an identifier (if it is a valid identifier, of course). Compare with the old behavior:

```sql
CREATE TABLE ... ENGINE=FEDERATED CONNECTION='mysql://root@127.0.0.1';
```

where the value of the ENGINE attribute is specified not quoted, while the value of the CONNECTION is quoted.

When an attribute is set, it will be stored with the table definition and shown in the <code class="fixed" style="white-space:pre-wrap"><span class="k">SHOW</span> <span class="k">CREATE</span> <span class="k">TABLE</span><span class="p">;</span>
</code>. To remove an attribute from a table definition use <code class="fixed" style="white-space:pre-wrap"><span class="k">ALTER</span> <span class="k">TABLE</span>
</code> to set its value to a <code class="fixed" style="white-space:pre-wrap"><span class="k">DEFAULT</span>
</code>.

The values of unknown attributes or attributes with the illegal values cause an error by default. But with [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) one can change the storage engine and some previously valid attributes may become unknown — to the new engine. They are not removed automatically, though, because the table might be altered back to the first engine, and these attributes will be valid again. Still [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) will comment these unknown attributes out in the output, otherwise they would make a generated [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) statement invalid.

With the <code class="fixed" style="white-space:pre-wrap"><span class="n">IGNORE_BAD_TABLE_OPTIONS</span>
</code> [sql mode](/kb/en/sql_mode/) this behavior changes. Unknown attributes do not cause an error, they only result in a warning. And [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) will not comment them out. This mode is implicitly enabled in the replication slave thread.

## See Also

- [Writing Plugins for MariaDB](/kb/en/writing-plugins-for-mariadb/)
- [Storage Engines](/columns-storage-engines-and-plugins/storage-engines/)
- [Storage Engine Development](/kb/en/storage-engine-development/)