# File Organization in PBXT

A short description of the files used by PBXT.

If you do the following create:

```sql
create table test.t9 (a int) engine=pbxt'.
```

You will find the following files in your database directory.

```sql
(/my/data) ls -Rl pbxt
pbxt:
total 32
-rw-rw---- 1 monty my    7 Nov  4 16:41 location
-rw-rw---- 1 monty my 8672 Nov  4 16:41 location.frm
-rw-rw---- 1 monty my 8687 Nov  4 16:41 statistics.frm
drwxrwxr-x 2 monty my 4096 Nov  4 16:41 system

pbxt/system:
total 32776
-rw-rw---- 1 monty my        9 Nov  4 16:41 ilog-1.xt
-rw-rw---- 1 monty my       44 Nov  4 16:41 restart-1.xt
-rw-rw---- 1 monty my 33554432 Nov  4 16:41 xlog-1.xt

(/my/data) ls -l test/t9*
-rw-rw---- 1 monty my    4 Nov  4 16:41 test/t9-1.xtr
-rw-rw---- 1 monty my 8554 Nov  4 16:41 test/t9.frm
-rw-rw---- 1 monty my  155 Nov  4 16:41 test/t9.xtd
-rw-rw---- 1 monty my 4096 Nov  4 16:41 test/t9.xti
```

TODO: Fill in a description of what the above files are used for.