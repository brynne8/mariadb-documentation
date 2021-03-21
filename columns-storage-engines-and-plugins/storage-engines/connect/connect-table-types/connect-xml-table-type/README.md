# CONNECT XML Table Type

## Overview

CONNECT supports tables represented by XML files. For these tables, the
standard input/output functions of the operating system are not used but the
parsing and processing of the file is delegated to a specialized library.
Currently two such systems are supported: libxml2, a part of the GNOME
framework, but which does not require GNOME and, on Windows, MS-DOM (DOMDOC), the Microsoft standard support of XML documents.

DOMDOC is the default for the Windows version of CONNECT and libxml2 is always
used on other systems. On Windows the choice can be specified using the XMLSUP
[CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) list option, for instance specifying
`option_list='xmlsup=libxml2'`.

## Creating XML tables

First of all, it must be understood that XML is a very general language used to
encode data having any structure. In particular, the tag hierarchy in an XML
file describes a tree structure of the data. For instance, consider the file:

```sql
<?xml version="1.0" encoding="ISO-8859-1"?>
<BIBLIO SUBJECT="XML">
   <BOOK ISBN="9782212090819" LANG="fr" SUBJECT="applications">
      <AUTHOR>
         <FIRSTNAME>Jean-Christophe</FIRSTNAME>
         <LASTNAME>Bernadac</LASTNAME>
      </AUTHOR>
      <AUTHOR>
         <FIRSTNAME>François</FIRSTNAME>
         <LASTNAME>Knab</LASTNAME>
      </AUTHOR>
      <TITLE>Construire une application XML</TITLE>
      <PUBLISHER>
         <NAME>Eyrolles</NAME>
         <PLACE>Paris</PLACE>
      </PUBLISHER>
      <DATEPUB>1999</DATEPUB>
   </BOOK>
   <BOOK ISBN="9782840825685" LANG="fr" SUBJECT="applications">
      <AUTHOR>
         <FIRSTNAME>William J.</FIRSTNAME>
         <LASTNAME>Pardi</LASTNAME>
      </AUTHOR>
      <TRANSLATOR PREFIX="adapté de l'anglais par">
         <FIRSTNAME>James</FIRSTNAME>
         <LASTNAME>Guerin</LASTNAME>
      </TRANSLATOR>
      <TITLE>XML en Action</TITLE>
      <PUBLISHER>
         <NAME>Microsoft Press</NAME>
         <PLACE>Paris</PLACE>
      </PUBLISHER>
      <DATEPUB>1999</DATEPUB>
   </BOOK>
</BIBLIO>
```

It represents data having the structure:

```sql
                               <BIBLIO>
                        __________|_________
                       |                    |
            <BOOK:ISBN,LANG,SUBJECT>        |
         ______________|_______________     |
        |        |         |           |    |
     <AUTHOR> <TITLE> <PUBLISHER> <DATEPUB> |
    ____|____            ___|____           |
   |    |    |          |        |          |
<FIRST> | <LAST>     <NAME>   <PLACE>       |
        |                                   |
     <AUTHOR>                   <BOOK:ISBN,LANG,SUBJECT>
    ____|____         ______________________|__________________
   |         |       |            |         |        |         |
<FIRST>   <LAST>  <AUTHOR> <TRANSLATOR> <TITLE> <PUBLISHER> <DATEPUB>
                _____|_        ___|___            ___|____
               |       |      |       |          |        |
            <FIRST> <LAST> <FIRST> <LAST>     <NAME>   <PLACE>
```

This structure seems at first view far from being tabular. However, modern
database management systems, including MariaDB, implement something close to the
relational model and work on tables that are structurally not hierarchical but
tabular with rows and columns.

Nevertheless, CONNECT can do it. Of course, it cannot guess what you want to
extract from the XML structure, but gives you the possibility to specify it
when you create the table<sup class="reference" id="_ref-0">[[1](#_note-0)]</sup>.

Let us take a first example. Suppose you want to make a table from the above
document, displaying the node contents.

For this, you can define a table <em>xsamptag</em> as:

```sql
create table xsamptag (
  AUTHOR char(50),
  TITLE char(32),
  TRANSLATOR char(40),
  PUBLISHER char(40),
  DATEPUB int(4))
engine=CONNECT table_type=XML file_name='Xsample.xml';
```

It will be displayed as:

<table><tbody><tr><th>AUTHOR</th><th>TITLE</th><th>TRANSLATOR</th><th>PUBLISHER</th><th>DATEPUB</th></tr>
<tr><td>Jean-Christophe Bernadac</td><td>Construire une application XML</td><td>&lt;null&gt;</td><td>Eyrolles Paris</td><td>1999</td></tr>
<tr><td>William J. Pardi</td><td>XML en Action</td><td>James Guerin</td><td>Microsoft Press Paris</td><td>1999</td></tr>
</tbody></table>

Let us try to understand what happened. By default the column names correspond
to tag names. Because this file is rather simple, CONNECT was able to default
the top tag of the table as the root node `&lt;BIBLIO&gt;` of the file, and the row
tags as the `&lt;BOOK&gt;` children of the table tag. In a more complex file, this
should have been specified, as we will see later. Note that we didn't have to worry
about the sub-tags such as `&lt;FIRSTNAME&gt;` or `&lt;LASTNAME&gt;` because CONNECT
automatically retrieves the entire text contained in a tag and its
sub-tags<sup class="reference" id="_ref-1">[[2](#_note-1)]</sup>.

Only the first author of the first book appears. This is because only the first
occurrence of a column tag has been retrieved so the result has a proper
tabular structure. We will see later what we can do about that.

How can we retrieve the values specified by attributes? By using a <em>Coltype</em>
table option to specify the default column type. The value ‘@’ means that
column names match attribute names. Therefore, we can retrieve them by creating
a table such as:

```sql
create table xsampattr (
  ISBN char(15),
  LANG char(2),
  SUBJECT char(32))
engine=CONNECT table_type=XML file_name='Xsample.xml'
option_list='Coltype=@';
```

This table returns the following:

<table><tbody><tr><th>ISBN</th><th>LANG</th><th>SUBJECT</th></tr>
<tr><td>9782212090819</td><td>fr</td><td>applications</td></tr>
<tr><td>9782840825685</td><td>fr</td><td>applications</td></tr>
</tbody></table>

Now to define a table that will give us all the previous information, we must
specify the column type for each column. Because in the next statement the
column type defaults to Node, the <em>field_format</em> column parameter was used to
indicate which columns are attributes:

```sql
create table xsamp (
  ISBN char(15) field_format='@',
  LANG char(2) field_format='@',
  SUBJECT char(32) field_format='@',
  AUTHOR char(50),
  TITLE char(32),
  TRANSLATOR char(40),
  PUBLISHER char(40),
  DATEPUB int(4))
engine=CONNECT table_type=XML file_name='Xsample.xml'
tabname='BIBLIO' option_list='rownode=BOOK';
```

Once done, we can enter the query:

```sql
select subject, lang, title, author from xsamp;
```

This will return the following result:

<table><tbody><tr><th>SUBJECT</th><th>LANG</th><th>TITLE</th><th>AUTHOR</th></tr>
<tr><td>applications</td><td>fr</td><td>Construire une application XML</td><td>Jean-Christophe Bernadac</td></tr>
<tr><td>applications</td><td>fr</td><td>XML en Action</td><td>William J. Pardi</td></tr>
</tbody></table>

Note that we have been lucky. Because unlike SQL, XML is case sensitive and the
column names have matched the node names only because the column names were given
in upper case. Note also that the order of the columns in the table could have
been different from the order in which the nodes appear in the XML file.

## Using Xpath with XML tables

Xpath is used by XML to locate and retrieve nodes. The table's main node Xpath is specified by the `tabname` option. If just the node name is given, CONNECT constructs an Xpath such as `‘<em>BIBLIO’</em>` in the example above that should retrieve the `BIBLIO` node wherever it is within the XML file.

The row nodes are by default the children of the table node. However, for instance to eliminate some children nodes that are not real row nodes, the row node name can be specified using the `rownode` sub-option of the `option_list` option.

The field_format options we used above can be specified to locate more
precisely where and what information to retrieve using an Xpath-like
syntax. For instance:

```sql
create table xsampall (
  isbn char(15) field_format='@ISBN',
  language char(2) field_format='@LANG',
  subject char(32) field_format='@SUBJECT',
  authorfn char(20) field_format='AUTHOR/FIRSTNAME',
  authorln char(20) field_format='AUTHOR/LASTNAME',
  title char(32) field_format='TITLE',
  translated char(32) field_format='TRANSLATOR/@PREFIX',
  tranfn char(20) field_format='TRANSLATOR/FIRSTNAME',
  tranln char(20) field_format='TRANSLATOR/LASTNAME',
  publisher char(20) field_format='PUBLISHER/NAME',
  location char(20) field_format='PUBLISHER/PLACE',
  year int(4) field_format='DATEPUB')
engine=CONNECT table_type=XML file_name='Xsample.xml'
tabname='BIBLIO' option_list='rownode=BOOK';
```

This very flexible column parameter serves several purposes:

- To specify the tag name, or the attribute name if different from the column
  name.
- To specify the type (tag or attribute) by a prefix of '@' for attributes.
- To specify the path for sub-tags using the '/' character.

This path is always relative to the current context (the column top node) and
cannot be specified as an absolute path from the document root, therefore a
leading '/' cannot be used. The path cannot be variable in node names or depth,
therefore using '`//`' is not allowed.

The query:

```sql
select isbn, title, translated, tranfn, tranln, location from
    xsampall where translated is not null;
```

replies:

<table><tbody><tr><th>ISBN</th><th>TITLE</th><th>TRANSLATED</th><th>TRANFN</th><th>TRANLN</th><th>LOCATION</th></tr>
<tr><td>9782840825685</td><td>XML en Action</td><td>adapté de l'anglais par</td><td>James</td><td>Guerin</td><td>Paris</td></tr>
</tbody></table>

### Libxml2 default name space issue

An issue with libxml2 is that some files can declare a default name space in their root node. Because Xpath only searches in that name space, the nodes will not be found if they are not prefixed. If this happens, specify the tabname option as an Xpath ignoring the current name space:

```sql
TABNAME="//*[local-name()='BIBLIO']"
```

This must also be done for the default of specified Xpath of the not attribute columns. For instance:

```sql
title char(32) field_format="*[local-name()='TITLE']",
```

Note: This raises an error (and is useless anyway) with DOMDOC.

### Direct access on XML tables

Direct access is available on XML tables. This means that XML tables can be
sorted and used in joins, even in the one-side of the join.

However, building a permanent index is not yet implemented. It is unclear whether this can be useful. Indeed, the DOM implementation that is used to access these tables firstly parses the whole file and constructs a node tree in memory. This may often be the longest part of the process, so the use of an index would not be of great value. Note also that this limits the XML files to a reasonable size. Anyway, when speed is important, this table type is not the best to use. Therefore, in these cases, it is probably better to convert the file to another type by inserting the XML table into another table of a more appropriate type for performance.

## Having Columns defined by Discovery

It is possible to let the MariaDB discovery process do the job of column specification. When columns are not defined in the [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) statement, CONNECT endeavours to analyze the XML file and to provide the column specifications. This is possible only for true XML tables, but not for HTML tables.

For instance, the <em>xsamp</em> table could have been created specifying:

```sql
create table xsamp
engine=CONNECT table_type=XML file_name='Xsample.xml'
tabname='BIBLIO' option_list='rownode=BOOK';
```

Let’s check how it was actually specified using the SHOW CREATE TABLE statement:

```sql
CREATE TABLE `xsamp` (
  `ISBN` char(13) NOT NULL `FIELD_FORMAT`='@',
  `LANG` char(2) NOT NULL `FIELD_FORMAT`='@',
  `SUBJECT` char(12) NOT NULL `FIELD_FORMAT`='@',
  `AUTHOR` char(24) NOT NULL,
  `TRANSLATOR` char(12) DEFAULT NULL,
  `TITLE` char(30) NOT NULL,
  `PUBLISHER` char(21) NOT NULL,
  `DATEPUB` char(4) NOT NULL
) ENGINE=CONNECT DEFAULT CHARSET=latin1 `TABLE_TYPE`='XML' 
  `FILE_NAME`='E:/Data/Xml/Xsample.xml' `TABNAME`='BIBLIO' `OPTION_LIST`='rownode=BOOK';
```

It is equivalent except for the column sizes that have been calculated from the file as the maximum length of the corresponding column when it was a normal value. Also, all columns are specified as type [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char) because XML does not provide information about the node content data type. Nullable is set to true if the column is missing in some rows.

If a more complex definition is desired, you can ask CONNECT to analyse the XPATH up to a given level using the level option in the option list. The level value is the number of nodes that are taken in the XPATH. For instance:

```sql
create table xsampall
engine=CONNECT table_type=XML file_name='Xsample.xml'
tabname='BIBLIO' option_list='rownode=BOOK,Level=1';
```

This will define the table as:

```sql
CREATE TABLE `xsampall` (
  `ISBN` char(13) NOT NULL `FIELD_FORMAT`='@',
  `LANG` char(2) NOT NULL `FIELD_FORMAT`='@',
  `SUBJECT` char(12) NOT NULL `FIELD_FORMAT`='@',
  `AUTHOR_FIRSTNAME` char(15) NOT NULL `FIELD_FORMAT`='AUTHOR/FIRSTNAME',
  `AUTHOR_LASTNAME` char(8) NOT NULL `FIELD_FORMAT`='AUTHOR/LASTNAME',
  `TRANSLATOR_PREFIX` char(24) DEFAULT NULL `FIELD_FORMAT`='TRANSLATOR/@PREFIX',
  `TRANSLATOR_FIRSTNAME` char(7) DEFAULT NULL `FIELD_FORMAT`='TRANSLATOR/FIRSTNAME',
  `TRANSLATOR_LASTNAME` char(6) DEFAULT NULL `FIELD_FORMAT`='TRANSLATOR/LASTNAME',
  `TITLE` char(30) NOT NULL,
  `PUBLISHER_NAME` char(15) NOT NULL `FIELD_FORMAT`='PUBLISHER/NAME',
  `PUBLISHER_PLACE` char(5) NOT NULL `FIELD_FORMAT`='PUBLISHER/PLACE',
  `DATEPUB` char(4) NOT NULL
) ENGINE=CONNECT DEFAULT CHARSET=latin1 `TABLE_TYPE`='XML' `FILE_NAME`='Xsample.xml' 
  `TABNAME`='BIBLIO' `OPTION_LIST`='rownode=BOOK,Level=1';
```

This method can be used as a quick way to make a “template” table definition that can later be edited to make the desired definition. In particular, column names are constructed from all the nodes of their path in order to have distinct column names. This can be manually edited to have the desired names, provided their XPATH is not modified.

To have a preview of how columns will be defined, you can use a catalog table like this:

```sql
create table xsacol
engine=CONNECT table_type=XML file_name='Xsample.xml'
tabname='BIBLIO' option_list='rownode=BOOK,Level=1' catfunc=col;
```

And when asking:

```sql
select column_name Name, type_name Type, column_size Size, nullable, xpath from xsacol;
```

You get the description of what the table columns will be:

<table><tbody><tr><th>Name</th><th>Type</th><th>Size</th><th>nullable</th><th>xpath</th></tr>
<tr><td>ISBN</td><td>CHAR</td><td>13</td><td>0</td><td>@</td></tr>
<tr><td>LANG</td><td>CHAR</td><td>2</td><td>0</td><td>@</td></tr>
<tr><td>SUBJECT</td><td>CHAR</td><td>12</td><td>0</td><td>@</td></tr>
<tr><td>AUTHOR_FIRSTNAME</td><td>CHAR</td><td>15</td><td>0</td><td>AUTHOR/FIRSTNAME</td></tr>
<tr><td>AUTHOR_LASTNAME</td><td>CHAR</td><td>8</td><td>0</td><td>AUTHOR/LASTNAME</td></tr>
<tr><td>TRANSLATOR_PREFIX</td><td>CHAR</td><td>24</td><td>1</td><td>TRANSLATOR/@PREFIX</td></tr>
<tr><td>TRANSLATOR_FIRSTNAME</td><td>CHAR</td><td>7</td><td>1</td><td>TRANSLATOR/FIRSTNAME</td></tr>
<tr><td>TRANSLATOR_LASTNAME</td><td>CHAR</td><td>6</td><td>1</td><td>TRANSLATOR/LASTNAME</td></tr>
<tr><td>TITLE</td><td>CHAR</td><td>30</td><td>0</td><td></td></tr>
<tr><td>PUBLISHER_NAME</td><td>CHAR</td><td>15</td><td>0</td><td>PUBLISHER/NAME</td></tr>
<tr><td>PUBLISHER_PLACE</td><td>CHAR</td><td>5</td><td>0</td><td>PUBLISHER/PLACE</td></tr>
<tr><td>DATEPUB</td><td>CHAR</td><td>4</td><td>0</td><td></td></tr>
</tbody></table>

## Create, Read, Update and Delete operations on XML tables

You can freely use the Update, Delete and Insert commands with XML tables.
However, you must understand that the format of the updated or inserted data
follows the specifications of the table you created, not the ones of the
original source file. For instance, let us suppose we insert a new book using
the <em>xsamp</em> table (not the <em>xsampall</em> table) with the command:

```sql
insert into xsamp
  (isbn, lang, subject, author, title, publisher,datepub)
  values ('9782212090529','fr','général','Alain Michard',
         'XML, Langage et Applications','Eyrolles Paris',1998);
```

Then if we ask:

```sql
select subject, author, title, translator, publisher from xsamp;
```

Everything seems correct when we get the result:

<table><tbody><tr><th>SUBJECT</th><th>AUTHOR</th><th>TITLE</th><th>TRANSLATOR</th><th>PUBLISHER</th></tr>
<tr><td>applications</td><td>Jean-Christophe Bernadac</td><td>Construire une application XML</td><td></td><td>Eyrolles Paris</td></tr>
<tr><td>applications</td><td>William J. Pardi</td><td>XML en Action</td><td>James Guerin</td><td>Microsoft Press Paris</td></tr>
<tr><td>général</td><td>Alain Michard</td><td>XML, Langage et Applications</td><td></td><td>Eyrolles Paris</td></tr>
</tbody></table>

However if we enter the apparently equivalent query on the <em>xsampall</em> table,
based on the same file:

```sql
select subject,
concat(authorfn, ' ', authorln) author , title,
concat(tranfn, ' ', tranln) translator,
concat(publisher, ' ', location) publisher from xsampall;
```

this returns an apparently wrong answer:

<table><tbody><tr><th>SUBJECT</th><th>AUTHOR</th><th>TITLE</th><th>TRANSLATOR</th><th>PUBLISHER</th></tr>
<tr><td>applications</td><td>Jean-Christophe Bernadac</td><td>Construire une application XML</td><td></td><td>Eyrolles Paris</td></tr>
<tr><td>applications</td><td>William J. Pardi</td><td>XML en Action</td><td>James Guerin</td><td>Microsoft Press Paris</td></tr>
<tr><td>général</td><td></td><td>XML, Langage et Applications</td><td></td><td></td></tr>
</tbody></table>

What happened here? Simply, because we used the <em>xsamp</em> table to do the
Insert, what has been inserted within the XML file had the structure described
for <em>xsamp</em>:

```sql
   <BOOK ISBN="9782212090529" LANG="fr" SUBJECT="général">
      <AUTHOR>Alain Michard</AUTHOR>
      <TITLE>XML, Langage et Applications</TITLE>
      <TRANSLATOR></TRANSLATOR>
      <PUBLISHER>Eyrolles Paris</PUBLISHER>
      <DATEPUB>1998</DATEPUB>
   </BOOK>
```

CONNECT cannot "invent" sub-tags that are not part of the <em>xsamp</em> table.
Because these sub-tags do not exist, the <em>xsampall</em> table cannot retrieve the
information that should be attached to them. If we want to be able to query the
XML file by all the defined tables, the correct way to insert a new book to the
file is to use the <em>xsampall</em> table, the only one that addresses all the
components of the original document:

```sql
delete from xsamp where isbn = '9782212090529';

insert into xsampall (isbn, language, subject, authorfn, authorln,
      title, publisher, location, year)
   values('9782212090529','fr','général','Alain','Michard',
      'XML, Langage et Applications','Eyrolles','Paris',1998);
```

Now the added book, in the XML file, will have the required structure:

```sql
   <BOOK ISBN="9782212090529" LANG="fr" SUBJECT="général"
      <AUTHOR>
         <FIRSTNAME>Alain</FIRSTNAME>
         <LASTNAME>Michard</LASTNAME>
      </AUTHOR>
      <TITLE>XML, Langage et Applications</TITLE>
      <PUBLISHER>
         <NAME>Eyrolles</NAME>
         <PLACE>Paris</PLACE>
      </PUBLISHER>
      <DATEPUB>1998</DATEPUB>
   </BOOK>
```

<strong>Note:</strong> We used a column list in the Insert statements when creating the table to avoid generating a `&lt;TRANSLATOR&gt;`
node with sub-nodes, all containing null values (this works on Windows only).

## Multiple nodes in the XML document

Let us come back to the above example XML file. We have seen that the author
node can be "multiple" meaning that there can be more than one author of a
book. What can we do to get the complete information fitting the relational
model? CONNECT provides you with two possibilities, but is restricted to only one
such multiple node per table.

The first and most challenging one is to return as many rows than there are
authors, the other columns being repeated as if we had make a join between the
author column and the rest of the table. To achieve this, simply specify the
“multiple” node name and the “expand” option when creating the table. For
instance, we can create the <em>xsamp2</em> table like this:

```sql
create table xsamp2 (
  ISBN char(15) field_format='@',
  LANG char(2) field_format='@',
  SUBJECT char(32) field_format='@',
  AUTHOR char(40),
  TITLE char(32),
  TRANSLATOR char(32),
  PUBLISHER char(32),
  DATEPUB int(4))
engine=CONNECT table_type=XML file_name='Xsample.xml'
tabname='BIBLIO'
option_list='rownode=BOOK,Expand=1,Mulnode=AUTHOR,Limit=2';
```

In this statement, the Limit option specifies the maximum number of values that will be expanded. If not specified, it defaults to `10`. Any values above the limit will be ignored and a warning message issued<sup class="reference" id="_ref-2">[[3](#_note-2)]</sup>. Now you can enter a query such as:

```sql
select isbn, subject, author, title from xsamp2;
```

This will retrieve and display the following result:

<table><tbody><tr><th>ISBN</th><th>SUBJECT</th><th>AUTHOR</th><th>TITLE</th></tr>
<tr><td>9782212090819</td><td>applications</td><td>Jean-Christophe Bernadac</td><td>Construire une application XML</td></tr>
<tr><td>9782212090819</td><td>applications</td><td>François Knab</td><td>Construire une application XML</td></tr>
<tr><td>9782840825685</td><td>applications</td><td>William J. Pardi</td><td>XML en Action</td></tr>
<tr><td>9782212090529</td><td>général</td><td>Alain Michard</td><td>XML, Langage et Applications</td></tr>
</tbody></table>

In this case, this is as if the table had four rows. However if we enter the query:

```sql
select isbn, subject, title, publisher from xsamp2;
```

this time the result will be:

<table><tbody><tr><th>ISBN</th><th>SUBJECT</th><th>TITLE</th><th>PUBLISHER</th></tr>
<tr><td>9782212090819</td><td>applications</td><td>Construire une application XML</td><td>Eyrolles Paris</td></tr>
<tr><td>9782840825685</td><td>applications</td><td>XML en Action</td><td>Microsoft Press Paris</td></tr>
<tr><td>9782212090529</td><td>général</td><td>XML, Langage et Applications</td><td>Eyrolles Paris</td></tr>
</tbody></table>

Because the author column does not appear in the query, the corresponding row
was not expanded.  This is somewhat strange because this would have been
different if we had been working on a table of a different type. However, it is
closer to the relational model for which there should not be two identical rows
(tuples) in a table. Nevertheless, you should be aware of this somewhat erratic
behavior. For instance:

```sql
select count(*) from xsamp2;                /* Replies 3 */
select count(author) from xsamp2;           /* Replies 4 */
select count(isbn) from xsamp2;             /* Replies 3 */
select isbn, subject, title, publisher from xsamp2 where author <> '';
```

This last query replies:

<table><tbody><tr><th>ISBN</th><th>SUBJECT</th><th>TITLE</th><th>PUBLISHER</th></tr>
<tr><td>9782212090819</td><td>applications</td><td>Construire une application XML</td><td>Eyrolles Paris</td></tr>
<tr><td>9782212090819</td><td>applications</td><td>Construire une application XML</td><td>Eyrolles Paris</td></tr>
<tr><td>9782840825685</td><td>applications</td><td>XML en Action</td><td>Microsoft Press Paris</td></tr>
<tr><td>9782212090529</td><td>général</td><td>XML, Langage et Applications</td><td>Eyrolles Paris</td></tr>
</tbody></table>

Even though the author column does not appear in the result, the corresponding row was
expanded because the multiple column was used in the where clause.

## Intermediate multiple node

The "multiple" node can be an intermediate node. If we want to do the same
expanding with the <em>xsampall</em> table, there will be nothing more to do. The
 <em>xsampall2</em> table can be created with:

```sql
create table xsampall2 (
  isbn char(15) field_format='@ISBN',
  language char(2) field_format='@LANG',
  subject char(32) field_format='@SUBJECT',
  authorfn char(20) field_format='AUTHOR/FIRSTNAME',
  authorln char(20) field_format='AUTHOR/LASTNAME',
  title char(32) field_format='TITLE',
  translated char(32) field_format='TRANSLATOR/@PREFIX',
  tranfn char(20) field_format='TRANSLATOR/FIRSTNAME',
  tranln char(20) field_format='TRANSLATOR/LASTNAME',
  publisher char(20) field_format='PUBLISHER/NAME',
  location char(20) field_format='PUBLISHER/PLACE',
  year int(4) field_format='DATEPUB')
engine=CONNECT table_type=XML file_name='Xsample.xml'
tabname='BIBLIO'
option_list='rownode=BOOK,Expand=1,Mulnode=AUTHOR,Limit=2';
```

The only difference is that the "multiple" node is an intermediate node in the
path. The resulting table can be seen with a query such as:

```sql
select subject, language lang, title, authorfn first, authorln
    last, year from xsampall2;
```

This query displays:

<table><tbody><tr><th>SUBJECT</th><th>LANG</th><th>TITLE</th><th>FIRST</th><th>LAST</th><th>YEAR</th></tr>
<tr><td>applications</td><td>fr</td><td>Construire une application XML</td><td>Jean-Christophe</td><td>Bernadac</td><td>1999</td></tr>
<tr><td>applications</td><td>fr</td><td>Construire une application XML</td><td>François</td><td>Knab</td><td>1999</td></tr>
<tr><td>applications</td><td>fr</td><td>XML en Action</td><td>William J.</td><td>Pardi</td><td>1999</td></tr>
<tr><td>général</td><td>fr</td><td>XML, Langage et Applications</td><td>Alain</td><td>Michard</td><td>1998</td></tr>
</tbody></table>

These composite tables, half array half tree, reserve some surprises for us when
updating, deleting from or inserting into them. Insert just cannot generate
this structure; if two rows are inserted with just a different author, two book
nodes will be generated in the XML file. Delete always deletes one book node
and all its children nodes even if specified against only one author. Update is
more complicated:

```sql
update xsampall2 set authorfn = 'Simon' where authorln = 'Knab';
update xsampall2 set year = 2002 where authorln = 'Bernadac';
update xsampall2 set authorln = 'Mercier' where year = 2002;
```

After these three updates, the first two responding "Affected rows: 1" and the
last one responding "Affected rows: 2", the last query answers:

<table><tbody><tr><th>subject</th><th>lang</th><th>title</th><th>first</th><th>last</th><th>year</th></tr>
<tr><td>applications</td><td>fr</td><td>Construire une application XML</td><td>Jean-Christophe</td><td>Mercier</td><td>2002</td></tr>
<tr><td>applications</td><td>fr</td><td>Construire une application XML</td><td>François</td><td>Knab</td><td>2002</td></tr>
<tr><td>applications</td><td>fr</td><td>XML en Action</td><td>William J.</td><td>Pardi</td><td>1999</td></tr>
<tr><td>général</td><td>fr</td><td>XML, Langage et Applications</td><td>Alain</td><td>Michard</td><td>1998</td></tr>
</tbody></table>

What must be understood here is that the Update modifies node values in the XML file, not cell values in the relational table. The first update worked normally. The second update changed the year value of the book and this shows for the two expanded rows because there is only one DATEPUB node for that book. Because the third update applies to a row having a certain date value, both author names were updated.

## Making a list of multiple values

Another way to see multiple values is to ask CONNECT to make a comma separated
list of the multiple node values. This time, it can only be done if the
"multiple" node is not intermediate. For example, we can modify the <em>xsamp2</em>
table definition by:

```sql
alter table xsamp2 option_list='rownode=BOOK,Mulnode=AUTHOR,Limit=3';
```

This time 'Expand' is not specified, and Limit gives the maximum number of
items in the list. Now if we enter the query:

```sql
select isbn, subject, author "AUTHOR(S)", title from xsamp2;
```

We will get the following result:

<table><tbody><tr><th>ISBN</th><th>SUBJECT</th><th>AUTHOR(S)</th><th>TITLE</th></tr>
<tr><td>9782212090819</td><td>applications</td><td>Jean-Christophe Bernadac, François Knab</td><td>Construire une application XML</td></tr>
<tr><td>9782840825685</td><td>applications</td><td>William J. Pardi</td><td>XML en Action</td></tr>
<tr><td>9782212090529</td><td>général</td><td>Alain Michard</td><td>XML, Langage et Applications</td></tr>
</tbody></table>

Note that updating the "multiple" column is not possible because CONNECT does
not know which of the nodes to update.

This could not have been done with the <em>xsampall2</em> table because the author
node is intermediate in the path, and making two lists, one of first names and
another one of last names would not make sense anyway.

### What if a table contains several multiple nodes

This can be handled by creating several tables on the same file, each
containing only one multiple node and constructing the desired result using
joins.

## Support of HTML tables

Most tables included in HTML documents cannot be processed by CONNECT because the HTML
language is often not compatible with the syntax of XML. In particular, XML
requires all open tags to be matched by a closing tag while it is sometimes
optional in HTML. This is often the case concerning column tags.

However, you can meet tables that respect the XML syntax but have some of the
features of HTML tables. For instance:

```sql
<?xml version="1.0"?>
<Beers>
  <table>
    <th><td>Name</td><td>Origin</td><td>Description</td></th>
    <tr>
      <td><brandName>Huntsman</brandName></td>
      <td><origin>Bath, UK</origin></td>
      <td><details>Wonderful hop, light alcohol</details></td>
    </tr>
    <tr>
      <td><brandName>Tuborg</brandName></td>
      <td><origin>Danmark</origin></td>
      <td><details>In small bottles</details></td>
    </tr>
  </table>
</Beers>
```

Here the different column tags are included in `&lt;td&gt;&lt;/td&gt;` tags as for HTML
tables. You cannot just add this tag in the Xpath of the columns, because the
search is done on the first occurrence of each tag, and this would cause this
search to fail for all columns except the first one. This case is handled by
specifying the <em>Colnode</em> table option that gives the name of these column
tags, for example:

```sql
create table beers (
  `Name` char(16) field_format='brandName',
  `Origin` char(16) field_format='origin',
  `Description` char(32) field_format='details')
engine=CONNECT table_type=XML file_name='beers.xml'
tabname='table' option_list='rownode=tr,colnode=td';
```

The table will be displayed as:

<table><tbody><tr><th>Name</th><th>Origin</th><th>Description</th></tr>
<tr><td>Huntsman</td><td>Bath, UK</td><td>Wonderful hop, light alcohol</td></tr>
<tr><td>Tuborg</td><td>Danmark</td><td>In small bottles</td></tr>
</tbody></table>

However, you can deal with tables even closer to the HTML model. For example
the <em>coffee.htm</em> file:

```sql
<TABLE summary="This table charts the number of cups of coffe
                consumed by each senator, the type of coffee (decaf
                or regular), and whether taken with sugar.">
  <CAPTION>Cups of coffee consumed by each senator</CAPTION>
  <TR>
    <TH>Name</TH>
    <TH>Cups</TH>
    <TH>Type of Coffee</TH>
    <TH>Sugar?</TH>
  </TR>
  <TR>
    <TD>T. Sexton</TD>
    <TD>10</TD>
    <TD>Espresso</TD>
    <TD>No</TD>
  </TR>
  <TR>
    <TD>J. Dinnen</TD>
    <TD>5</TD>
    <TD>Decaf</TD>
    <TD>Yes</TD>
  </TR>
</TABLE>
```

Here column values are directly represented by the TD tag text. You cannot
declare them as tags nor as attributes. In addition, they are not located using
their name but by their position within the row. Here is how to declare such a
table to CONNECT:

```sql
create table coffee (
  `Name` char(16),
  `Cups` int(8),
  `Type` char(16),
  `Sugar` char(4))
engine=connect table_type=XML file_name='coffee.htm'
tabname='TABLE' header=1 option_list='Coltype=HTML';
```

You specify the fact that columns are located by position by setting the
 <em>Coltype</em> option to 'HTML'. Each column position (0 based) will be the value
of the <em>flag</em> column parameter that is set by default in sequence. Now we are
able to display the table:

<table><tbody><tr><th>Name</th><th>Cups</th><th>Type</th><th>Sugar</th></tr>
<tr><td>T. Sexton</td><td>10</td><td>Espresso</td><td>No</td></tr>
<tr><td>J. Dinnen</td><td>5</td><td>Decaf</td><td>Yes</td></tr>
</tbody></table>

<strong>Note 1:</strong> We specified '`header=n`' in the create statement to indicate
that the first n rows of the table are not data rows and should be skipped.

<strong>Note 2:</strong> In this last example, we did not specify the node names using the
Rownode and Colnode options because when <em>Coltype</em> is set to 'HTML' they
default to '`Rownode=TR`' and '`Colnode=TD`'.

<strong>Note 3:</strong> The <em>Coltype</em> option is a word only the first character of which
is significant. Recognized values are:

<table><tbody><tr><td>T(ag) or N(ode)</td><td>Column names match a tag name (the default).</td></tr>
<tr><td>A(ttribute) or @</td><td>Column names match an attribute name.</td></tr>
<tr><td>H(tml) or C(ol) or P(os)</td><td>Column are retrieved by their position.</td></tr>
</tbody></table>

### New file setting

Some create options are used only when creating a table on a new file, i. e.
when inserting into a file that does not exist yet. When specified, the
'Header' option will create a header row with the name of the table columns.
This is chiefly useful for HTML tables to be displayed on a web browser.

Some new list-options are used in this context:

<table><tbody><tr><td><strong>Encoding</strong></td><td>The encoding of the new document, defaulting to UTF-8.</td></tr>
<tr><td><strong>Attribute</strong></td><td>A list of 'attname=attvalue' separated by ';' to add to the table node.</td></tr>
<tr><td><strong>HeadAttr</strong></td><td>An attribute list to be added to the header row node.</td></tr>
</tbody></table>

Let us see for instance, the following create statement:

```sql
create table handlers (
  handler char(64),
  version char(20),
  author char(64),
  description char(255),
  maturity char(12))
engine=CONNECT table_type=XML file_name='handlers.htm'
tabname='TABLE' header=yes
option_list='coltype=HTML,encoding=ISO-8859-1,
attribute=border=1;cellpadding=5,headattr=bgcolor=yellow';
```

Supposing the table file does not exist yet, the first insert into that table,
for instance by the following statement:

```sql
insert into handlers select plugin_name, plugin_version,
  plugin_author, plugin_description, plugin_maturity from
  information_schema.plugins where plugin_type = 'DAEMON';
```

will generate the following file:

```sql
<?xml version="1.0" encoding="ISO-8859-1"?>
<!-- Created by CONNECT Version 3.05.0005 August 17, 2012 -->
<TABLE border="1" cellpadding="5">
  <TR bgcolor="yellow">
    <TH>handler</TH>
    <TH>version</TH>
    <TH>author</TH>
    <TH>description</TH>
    <TH>maturity</TH>
  </TR>
  <TR>
    <TD>Maria</TD>
    <TD>1.5</TD>
    <TD>Monty Program Ab</TD>
    <TD>Compatibility aliases for the Aria engine</TD>
    <TD>Gamma</TD>
  </TR>
</TABLE>
```

This file can be used to display the table on a web browser (encoding should be
`ISO-8859-x`)

<table><tbody><tr><th>handler</th><th>version</th><th>author</th><th>description</th><th>maturity</th></tr>
<tr><td>Maria</td><td>1.5</td><td>Monty Program Ab</td><td>Compatibility aliases for the Aria engine</td><td>Gamma</td></tr>
</tbody></table>

<strong>Note:</strong> The XML document encoding is generally specified in the XML header
node and can be different from the DATA_CHARSET, which is always UTF-8 for XML
tables. Therefore the table DATA_CHARSET character set should be unspecified,
or specified as UTF8. The Encoding specification is useful only for new XML
files and ignored for existing files having their encoding already specified in
the header node.

## Notes

1 [↑](#_ref-0) CONNECT does not claim to be able to deal with
any XML document. Besides, those that can usefully be processed for data
analysis are likely to have a structure that can easily be transformed into a
table.
2 [↑](#_ref-1) With libxml2, sub tags text can be separated by 0 or several
blanks depending on the structure and indentation of the data file.
3 [↑](#_ref-2) This may cause some rows to be lost because an eventual where clause on the “multiple” column is applied only on the limited number of retrieved rows.