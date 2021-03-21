# Segmented Key Cache

## About Segmented Key Cache

A segmented key cache is a collection of structures for regular [MyISAM](/kb/en/myisam/)
key caches called key cache segments. Segmented key caches mitigate one
of the major problems of the simple key cache: thread contention for key
cache lock (mutex). With regular key caches, every call of a key cache
interface function must acquire this lock. So threads compete for this
lock even in the case when they have acquired shared locks for the file
and the pages they want to read from are in the key cache buffers.

When working with a segmented key cache any key cache interface
function that needs only one page has to acquire the key cache lock
only for the segment the page is assigned to. This makes the chances
for threads not having to compete for the same key cache lock better.

Any page from a file can be placed into a buffer of only one segment.
The number of the segment is calculated from the file number and the
position of the page in the file, and it's always the same for the
page. Pages are evenly distributed among segments.

The idea and the original code of the segmented key cache was provided
by Fredrik Nylander from Stardoll.com. The code was extensively
reworked, improved, and eventually merged into MariaDB by Igor Babaev
from Monty Program (now MariaDB Corporation).

You can find some benchmark results comparing various settings on the [Segmented Key Cache Performance](/kb/en/segmented-key-cache-performance/) page.

## Segmented Key Cache Syntax

New global variable: [key_cache_segments](/kb/en/myisam-system-variables/#key_cache_segments). This variable sets the number of segments in a key cache. Valid values for this variable are whole
numbers between `0` and `64`. If the number of segments is set to a number
greater than `64` the number of segments will be truncated to 64 and a warning will be issued.

A value of `0` means the key cache is a regular (i.e. non-segmented)
key cache. This is the default. If `key_cache_segments` is 
`1` (or higher) then the new key cache segmentation code is used. In
practice there is no practical use of a single-segment segmented key cache except
for testing purposes, and setting 
<code class="fixed" style="white-space:pre-wrap">key_cache_segments = 1</code> should be slower than any other option and 
should not be used in production.

Other global variables used when working with regular key caches also
apply to segmented key caches: [key_buffer_size](/kb/en/myisam-system-variables/#key_buffer_size),
[key_cache_age_threshold](/kb/en/myisam-system-variables/#key_cache_age_threshold), [key_cache_block_size](/kb/en/myisam-system-variables/#key_cache_block_size), and
[key_cache_division_limit](/kb/en/myisam-system-variables/#key_cache_division_limit).

## Segmented Key Cache Statistics

Statistics about the key cache can be found by looking at the
[KEY_CACHES](/kb/en/information-schema-key_caches-table/) table in the [INFORMATION_SCHEMA](/kb/en/information_schema/)
database. Columns in this table are:

<table><tbody><tr><th>Column Name</th><th>Description</th></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">KEY_CACHE_NAME</code></td><td>The name of the key cache</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">SEGMENTS</code></td><td>total number of segments (set to <code class="fixed" style="white-space:pre-wrap">NULL</code> for regular key caches)</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">SEGMENT_NUMBER</code></td><td>segment number (set to <code class="fixed" style="white-space:pre-wrap">NULL</code> for any regular key caches and for rows containing aggregation statistics for segmented key caches)</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">FULL_SIZE</code></td><td>memory for cache buffers/auxiliary structures</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">BLOCK_SIZE</code></td><td>size of the blocks</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">USED_BLOCKS</code></td><td>number of currently used blocks</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">UNUSED_BLOCKS</code></td><td>number of currently unused blocks</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">DIRTY_BLOCKS</code></td><td>number of currently dirty blocks</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">READ_REQUESTS</code></td><td>number of read requests</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">READS</code></td><td>number of actual reads from files into buffers</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">WRITE_REQUESTS</code></td><td>number of write requests</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">WRITES</code></td><td>number of actual writes from buffers into files</td></tr>
</tbody></table>

## See Also

- [Segmented Key Cache Performance](/kb/en/segmented-key-cache-performance/)