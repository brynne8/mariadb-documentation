# SQLSTATE

SQLSTATE is a code which identifies SQL error conditions. It composed by five characters, which can be numbers or uppercase ASCII letters. An SQLSTATE value consists of a class (first two characters) and a subclass (last three characters).

There are three important standard classes. They all indicate in which logical group of errors the condition falls. They match to a particular keyword which can be used with [DECLARE HANDLER](/programming-customizing-mariadb/programmatic-compound-statements/declare-handler). Also, the SQLSTATE class determines the default value for the MYSQL_ERRNO and MESSAGE_TEXT condition properties.

- '00' means 'success'. It can not be set in any way, and can only be read via the API.
- '01' contains all warnings, and matches to the SQLWARNING keyword. The default MYSQL_ERRNO is 1642 and default MESSAGE_TEXT is 'Unhandled user-defined warning condition'.
- '02' is the NOT FOUND class. The default MYSQL_ERRNO is 1643 and default MESSAGE_TEXT is 'Unhandled user-defined not found condition'.
- All other classes match the SQLEXCEPTION keyword. The default MYSQL_ERRNO is 1644 and default MESSAGE_TEXT is 'Unhandled user-defined exception condition'.

The subclass, if it is set, indicates a particular condition, or a particular group of conditions within the class. However the '000' sequence means 'no subclass'.

For example, if you try to [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) from a table which does not exist, a 1109 error is produced, with a '42S02' SQLSTATE. '42' is the class and 'S02' is the subclass. This value matches to the SQLEXCEPTION keyword. When FETCH is called for a [cursor](/kb/en/programmatic-and-compound-statements-cursors/) which has already reached the end, a 1329 error is produced, with a '02000' SQLSTATE. The class is '02' and there is no subclass (because '000' means 'no subclass'). It can be handled by a NOT FOUND handlers.

The standard SQL specification says that classes beginning with 0, 1, 2, 3, 4, A, B, C, D, E, F and G are reserved for standard-defined classes, while other classes are vendor-specific. It also says that, when the class is standard-defined, subclasses starting with those characters (except for '000') are standard-defined subclasses, while other subclasses are vendor-defined. However, MariaDB and MySQL do not strictly obey this rule.

To read the SQLSTATE of a particular condition which is in the [diagnostics area](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/diagnostics-area), the [GET DIAGNOSTICS](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/get-diagnostics) statement can be used: the property is called RETURNED_SQLSTATE. For user-defined conditions ([SIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/signal) and [RESIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/resignal) statements), a SQLSTATE value must be set via the SQLSTATE clause. However, [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings) and [SHOW ERRORS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-errors) do not display the SQLSTATE.

For user-defined conditions, MariaDB and MySQL recommend the '45000' SQLSTATE class.

'HY000' is called the "general error": it is the class used for builtin conditions which do not have a specific SQLSTATE class.

A partial list of error codes and matching SQLSTATE values can be found in the page [MariaDB Error Codes](/sql-statements-structure/sql-language-structure/mariadb-error-codes).