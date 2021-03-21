# replace Utility

## Description

The replace utility program changes strings in place in
files or on the standard input. Invoke replace in one of the
following ways:

```sql
shell> replace from to [from to] ... -- file_name [file_name] ...
shell> replace from to [from to] ... < file_name
```

"`from`" represents a string to look for and "`to`" represents its
replacement.  There can be one or more pairs of strings.

A from-string can contain these special characters:

<table><tbody><tr><th>Character</th><th>Description</th></tr>
<tr><td><code>\^</code></td><td>Match start of line.</td></tr>
<tr><td><code>\$</code></td><td>Match end of line.</td></tr>
<tr><td><code>\b</code></td><td>Match space-character, start of line or end of line. For an end <code>\b</code> the next replace starts looking at the end space-character. A <code>\b</code> alone in a string matches only a space-character</td></tr>
</tbody></table>

Use the `--` option to indicate where the string-replacement
list ends and the file names begin. Any file named on the command line is
modified in place, so you may want to make a copy of the original before
converting it. `replace` prints a message indicating which of the input
files it actually modifies.

If the `--` option is not given, replace reads standard
input and writes to standard output.

replace uses a finite state machine to match longer strings first. It can
be used to swap strings. For example, the following command swaps "a" and "b"
in the given files, /file1<em> and </em>file2<em>:</em>

```sql
shell> replace a b b a -- file1 file2 ...
```

The replace program is used by [msql2mysql](/clients-utilities/msql2mysql/).

## Options

`replace` supports the following options.

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>-?</code>, <code>-I</code></td><td>Display a help message and exit.</td></tr>
<tr><td><code>-#debug_options</code></td><td>Enable debugging.</td></tr>
<tr><td><code>-s</code></td><td>Silent mode. Print less information about what the program does.</td></tr>
<tr><td><code>-v</code></td><td>Verbose mode. Print more information about what the program does.</td></tr>
<tr><td><code>-V</code></td><td>Display version information and exit.</td></tr>
</tbody></table>