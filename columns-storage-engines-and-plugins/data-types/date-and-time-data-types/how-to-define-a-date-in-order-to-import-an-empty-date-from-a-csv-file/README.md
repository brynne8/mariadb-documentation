# How to define a date in order to import an empty date from a CSV file?

I have a CSV file containing amongst other things a couple of date columns. Some of the date values are empty - that is just "<sub>" - a comma followed by another comma.</sub>

The date rows in the table are defined like this:
	`PTY_USR_PASSWORD_DATE` DATE NULL DEFAULT NULL,

But when I import the data (using HeidiSQL), I get the following error:
/* SQL Error (1292): Incorrect date value: '' for column 'PTY_USR_PASSWORD_DATE' at row 1 */

How should I set up the table to allow this? Or how can I modify the CSV?

John D.

## Answer

That error is thrown when the [sql_mode](/kb/en/server-system-variables/#sql_mode) server system variable is set to strict mode. If it's not set, you can enter a blank date - a warning is generated and '0000-00-00' will be inserted.

If you can set strict mode off for the duration of the import, that should work, otherwise you'll need to replace the blank date columns in the CSV with NULL, or '0000-00-00'.