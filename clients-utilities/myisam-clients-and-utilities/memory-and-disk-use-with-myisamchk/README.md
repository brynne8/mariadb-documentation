# Memory and Disk Use With myisamchk

myisamchk's performance can be dramatically enhanced for larger tables by making sure that its memory-related variables are set to an optimum level.

By default, myisamchk will use very little memory (about 3MB is allocated), but can temporarily use a lot of disk space. If disk space is a limitation when repairing, the <em>--safe-recover</em> option should be used instead of <em>--recover</em>. However, if TMPDIR points to a memory file system, an out of memory error can easily be caused, as myisamchk places temporary files in TMPDIR. The <em>--tmpdir=path</em> option should be used in this case to specify a directory on disk.

myisamchk has the following requirements for disk space:

- When repairing, space for twice the size of the data file, available in the same directory as the original file. This is for the original file as well as a copy. This space is not required if the <em>--quick</em> option is used, in which case only the index file is re-created.
- Disk space in the temporary directory (TMPDIR or the <em>tmpdir=path</em> option) is needed for sorting if the <em>--recover</em> or <em>--sort-recover</em> options are used when not using <em>--safe-recover</em>). The space required will be approximately <em>(largest_key + row_pointer_length) * number_of_rows * 2</em>. To get information about the length of the keys as well as the row pointer length, use <em>myisamchk -dv table_name</em>.
- Space for a new index file to replace the existing one. The old index is first truncated, so unless the old index file is not present or is smaller for some reason, no significant extra space will be needed.

There are a number of [system variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) that are useful to adjust when running myisamchk. They will increase memory usage, and since some are per-session variables, you don't want to increase the general value, but you can either pass an increased value to myisamchk as a commandline option, or with a [myisamchk] section in your [my.cnf](/kb/en/configuring-mariadb-with-mycnf/) file.

- [sort_buffer_size](/kb/en/server-system-variables/#sort_buffer_size). By default this is 4M, but it's very useful to increase to make myisamchk sorting much faster. Since the server won't be running when you run myisamchk, you can increase substantially. 16M is usually a minimum, but values such as 256M are not uncommon if memory is available.
- [key_buffer_size](/kb/en/myisam-system-variables/#key_buffer_size) (which particularly helps with the <em>--extend-check</em> and <em>--safe-recover</em> options.
- [read_buffer_size](/kb/en/server-system-variables/#read_buffer_size)
- [write_buffer_size](/kb/en/server-system-variables/#write_buffer_size)

For example, if you have more than 512MB available to allocate to the process, the following settings could be used:

```sql
myisamchk 
  --myisam_sort_buffer_size=256M
  --key_buffer_size=512M
  --read_buffer_size=64M
  --write_buffer_size=64M
...
```