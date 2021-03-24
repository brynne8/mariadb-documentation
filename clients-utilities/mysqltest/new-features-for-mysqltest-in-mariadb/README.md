# New Features for mysqltest in MariaDB

Note that not all MariaDB-enhancements are listed on this page. See [mysqltest and mysqltest-embedded](/kb/en/mysqltest-and-mysqltest-embedded/) for a full set of options.

## Startup Option --connect-timeout

```sql
--connect-timeout=N
```

This can be used to set the MYSQL_OPT_CONNECT_TIMEOUT parameter of
mysql_options, to change the number of seconds before an unsuccessful
connection attempt times out.

## Test Commands for Handling Warnings During Prepare Statements

- <code class="highlight fixed" style="white-space:pre-wrap">enable_prepare_warnings;</code>
- <code class="highlight fixed" style="white-space:pre-wrap">disable_prepare_warnings;</code>

Normally, when running with the prepared statement protocol with warnings
enabled and executing a statement that returns a result set (like SELECT),
warnings that occur during the execute phase are shown, but warnings that occur
during the prepare phase are ''not'' shown. The reason for this is that some
warnings are returned both during prepare and execute; if both copies of
warnings were shown, then test cases would show different number of warnings
between prepared statement execution and normal execution (where there is no
prepare phase).

The <code class="highlight fixed" style="white-space:pre-wrap">enable_prepare_warnings</code> command changes this so that
warnings from both the prepare and execute phase are shown, regardless of
whether the statement produces a result set in the execute phase. The
<code class="highlight fixed" style="white-space:pre-wrap">disable_prepare_warnings</code> command reverts to the default
behaviour.

These commands only have effect when running with the prepared statement
protocol (--ps-protocol) <em>and</em> with warnings enabled
(enable_warnings). Furthermore, they only have effects for statements that
return a result set (as for statements without result sets, warnings from are
always shown when warnings are enabled).

##### MariaDB [10.0.13](/kb/en/mariadb-10013-release-notes/)

The `replace_regex` command supports paired delimiters (like in perl, etc). If the first non-space character in the `replace_regex` argument is one of `(`, `[`, `{`, `&lt;`, then the pattern should end with `)`, `]`, `}`, `&gt;` accordingly. The replacement string can use its own pair of delimiters, not necessarily the same as the pattern. If the first non-space character in the `replace_regex` argument is not one of the above, then it should also separate the pattern and the replacement string and it should end the replacement string. Backslash can be used to escape the current terminating character as usual. The examples below demonstrate valid usage of `replace_regex`:

```sql
--replace_regex (/some/path)</another/path>
--replace_regex !/foo/bar!foobar!
--replace_regex {pat\}tern}/replace\/ment/i
```