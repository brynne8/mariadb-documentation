# Using CONNECT - Importing File Data Into MariaDB Tables

Directly using external (file) data has many advantages, such as to work on “fresh” data produced for instance by cash registers, telephone switches, or scientific apparatus. However, you may want in some case to import external data into your MariaDB database. This is extremely simple and flexible using the CONNECT handler. For instance, let us suppose you want to import the data of the xsample.xml XML file previously given in example into a [MyISAM](/kb/en/myisam/) table called <em>biblio</em> belonging to the connect database. All you have to do is to create it by:

```sql
create table biblio engine=myisam select * from xsampall2;
```

This last statement creates the [MyISAM](/kb/en/myisam/) table and inserts the original XML data, translated to tabular format by the <em>xsampall2</em> CONNECT table, into the MariaDB <em>biblio</em> table. Note that further transformation on the data could have been achieved by using a more elaborate Select statement in the Create statement, for instance using filters, alias or applying functions to the data. However, because the Create Table process copies table data, later modifications of the <em>xsample.xml</em> file will not change the <em>biblio</em> table and changes to the <em>biblio</em> table will not modify the <em>xsample.xml</em> file.

All these can be combined or transformed by further SQL operations. This makes working with
CONNECT much more flexible than just using the [LOAD](/kb/en/load/) statement.