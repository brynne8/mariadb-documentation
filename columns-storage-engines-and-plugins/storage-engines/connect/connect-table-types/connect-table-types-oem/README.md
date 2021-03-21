# CONNECT Table Types - OEM: Implemented in an External LIB

Although CONNECT provides a rich set of table types, specific applications may
need to access data organized in a way that is not handled by its existing
foreign data wrappers (FDW). To handle these cases, CONNECT features an
interface that enables developers to implement in C++ the required table wrapper
and use it as if it were part of the standard CONNECT table type list. CONNECT
can use these additional handlers providing the corresponding external module
(dll or shared lib) be available.

To create such a table on an existing handler, use a Create Table statement as
shown below.

```sql
create table xtab (column definitions)
engine=CONNECT table_type=OEM module='libname'
subtype='MYTYPE' [standard table options]
Option_list='Myopt=foo';
```

The option module gives the name of the DLL or shared library implementing the OEM wrapper for the table type. This library must be located in the plugin directory like all other plugins or UDF’s.

This library must export a function <em>GetMYTYPE</em>. The option subtype enables CONNECT to have the name of the exported function and to use the new table type. Other options are interpreted by the OEM type and can also be specified within the <em>option_list</em> option.

Column definitions can be unspecified only if the external wrapper is able to
return this information. For this it must export a function ColMYTYPE returning
these definitions in a format acceptable by the CONNECT discovery function.

Which and how options must be specified and the way columns must be defined may
vary depending on the OEM type used and should be documented by the OEM type
implementer(s).

## An OEM Table Example

The OEM table REST described in [Adding the REST Feature as a Library Called by an OEM Table](/columns-storage-engines-and-plugins/storage-engines/connect/connect-adding-the-rest-feature-as-a-library-called-by-an-oem-table/) permits using REST-like tables with MariaDB binary distributions containing but not enabling the [REST table type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-files-retrieved-using-rest-queries/)

Of course, the mongo (dll or so) exporting the GetREST and colREST functions must be available in the plugin directory for all this to work.

### Some Currently Available OEM Table Modules and Subtypes

<table><tbody><tr><th>Module</th><th>Subtype</th><th>Description</th></tr>
<tr><td>libhello</td><td>HELLO</td><td>A sample OEM wrapper displaying a one line table saying “Hello world”</td></tr>
<tr><td>mongo</td><td>MONGO</td><td>Enables using tables based on MongoDB collections.</td></tr>
<tr><td>Tabfic</td><td>FIC</td><td>Handles files having the Windev HyperFile format.</td></tr>
<tr><td>Tabofx</td><td>OFC</td><td>Handles Open Financial Connectivity files.</td></tr>
<tr><td>Tabofx</td><td>QIF</td><td>Handles Quicken Interchange Format files.</td></tr>
<tr><td>Cirpack</td><td>CRPK</td><td>Handles CDR's from Cirpack UTP's.</td></tr>
<tr><td>Tabplg</td><td>PLG</td><td>Access tables from the PlugDB DBMS.</td></tr>
</tbody></table>

How to implement an OEM handler is out of the scope of this document.