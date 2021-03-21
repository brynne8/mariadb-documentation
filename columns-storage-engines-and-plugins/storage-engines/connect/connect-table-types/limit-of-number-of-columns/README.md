# limit of number of columns

I have a table of 6MB with 50 values in the pivot-values leading to 50 columns in the pivoted table. That one works nicely.

I have another much larger table with 170MB and 121 columns. When creating the pivoted table I get an error:

SQL Fehler (1939): Engine CONNECT failed to discover table `abc`.`def` with 'CREATE TABLE whatever ...

The server runs on Win7 64Bit version 10.0.13

How can I create the pivoted table of the large table?

## Answer
            <span class="answer_comment">Answered by 
<span class="user" id="user-1085">
[Daniel Black](/kb/user/id/1085)
</span> in [this comment](#comment_1412).</span>

the error sounds more like the table name is incorrect.

Alternately try the tracing documented here: [loading-the-connect-storage-engine](/kb/en/loading-the-connect-storage-engine/)