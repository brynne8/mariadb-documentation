# Optimizing key_buffer_size

[key_buffer_size](/kb/en/myisam-system-variables/#key_buffer_size) is a [MyISAM](/kb/en/myisam/) variable which determines the size of the index buffers held in memory, which affects the speed of index reads. Note that Aria tables by default make use of an alternative setting, [aria-pagecache-buffer-size](/kb/en/aria-server-system-variables/#aria_pagecache_buffer_size).

A good rule of thumb for servers consisting particularly of MyISAM tables is for about 25% or more of the available server memory to be dedicated to the key buffer.

A good way to determine whether to adjust the value is to compare the [key_read_requests](/kb/en/server-status-variables/#key_read_requests) value, which is the total value of requests to read an index, and the [key_reads](/kb/en/server-status-variables/#key_reads) values, the total number of requests that had to be read from disk.

The ratio of key_reads to key_read_requests should be as low as possible, 1:100 is the highest acceptable, 1:1000 is better, and 1:10 is terrible.

The effective maximum size might be lower than what is set, depending on the server's available physical RAM and the per-process limit determined by the operating system.

If you don't make use of MyISAM tables at all, you can set this to a very low value, such as 64K.