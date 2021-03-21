# resolve_stack_dump

resolve_stack_dump is a tool that resolves numeric stack strace dumps into symbols.

## Usage

```sql
resolve_stack_dump [OPTIONS] symbols-file [numeric-dump-file]
```

The symbols-file should include the output from:  `nm --numeric-sort mysqld`.
The numeric-dump-file should contain a numeric stack trace from mysqld.
If the numeric-dump-file is not given, the stack trace is read from stdin.

## Options

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>-h</code>, <code>--help</code></td><td>Display this help and exit.</td></tr>
<tr><td><code>-V</code>, <code>--version</code></td><td>Output version information and exit.</td></tr>
<tr><td><code>-s</code>, <code>--symbols-file=name</code></td><td>Use specified symbols file.</td></tr>
<tr><td><code>-n</code>, <code>--numeric-dump-file=name</code></td><td>Read the dump from specified file.</td></tr>
</tbody></table>