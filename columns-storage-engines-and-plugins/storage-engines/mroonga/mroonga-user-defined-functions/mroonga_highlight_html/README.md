# mroonga_highlight_html

## Syntax

```sql
mroonga_highlight_html(text[[, query AS query]])

mroonga_highlight_html(text[[, keyword1, ..., keywordN]])
```

## Description

`mroonga_highlight_html` is a [user-defined function](/programming-customizing-mariadb/user-defined-functions) (UDF) included with the [Mroonga storage engine](/columns-storage-engines-and-plugins/storage-engines/mroonga). It highlights the specified keywords in the target text. See [Creating Mroonga User-Defined Functions](/columns-storage-engines-and-plugins/storage-engines/mroonga/mroonga-user-defined-functions/creating-mroonga-user-defined-functions) for details on creating this UDF if required.

The optional parameter can either be one or more <em>keywords</em>, or a Groonga <em>query</em>.

The function highlights the specified keywords in the target text by surrounding each keyword with `&lt;span class="keyword"&gt;...&lt;/span&gt;`, and escaping special HTML characters such as `&lt;` and `&gt;`.

Returns highlighted HTML.

## Examples

```sql
SELECT mroonga_highlight_html('<p>MariaDB includes the Mroonga storage engine</p>.') 
  AS highlighted_html;
+-----------------------------------------------------------------+
| highlighted_html                                                |
+-----------------------------------------------------------------+
| &lt;p&gt;MariaDB includes the Mroonga storage engine&lt;/p&gt;. |
+-----------------------------------------------------------------+
```

Highlighting the words `MariaDB` and `Mroonga` in a given text:

```sql
SELECT mroonga_highlight_html('MariaDB includes the Mroonga storage engine.', 'MariaDB', 'Mroonga') 
  AS highlighted_html;
+--------------------------------------------------------------------------------------------------------+
| highlighted_html                                                                                       |
+--------------------------------------------------------------------------------------------------------+
| <span class="keyword">MariaDB</span> includes the <span class="keyword">Mroonga</span> storage engine. |
+--------------------------------------------------------------------------------------------------------+
```

The same outcome, formulated as a Groonga query:

```sql
SELECT mroonga_highlight_html('MariaDB includes the Mroonga storage engine.', 'MariaDB OR Mroonga' 
  AS query) AS highlighted_text;
+--------------------------------------------------------------------------------------------------------+
| highlighted_text                                                                                       |
+--------------------------------------------------------------------------------------------------------+
| <span class="keyword">MariaDB</span> includes the <span class="keyword">Mroonga</span> storage engine. |
+--------------------------------------------------------------------------------------------------------+
```

## See Also

- [Creating Mroonga User-Defined Functions](/columns-storage-engines-and-plugins/storage-engines/mroonga/mroonga-user-defined-functions/creating-mroonga-user-defined-functions)