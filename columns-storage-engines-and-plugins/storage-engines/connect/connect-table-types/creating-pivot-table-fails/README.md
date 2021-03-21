# creating pivot table fails

I tried to create a pivot table based on an existing table "test1" and get an error. I am logged in and I can use "test1", so this error makes no sense to me. What I am doing wrong? Any hints?

MariaDB [nav]&gt; create table xsort engine=connect table_type=pivot tabname=test1;

ERROR 1105 (HY000): (1045) Access denied for user 'root'@'localhost' (using password: NO)

MariaDB [nav]&gt;

## Answer
            <span class="answer_comment">Answered by 
<span class="user" id="user-27">
[Sergei Golubchik](/kb/user/id/27)
</span> in [this comment](#comment_1124).</span>

if you think it's a bug, please, report it on mariadb.org/jira and you can expect it to be fixed in the next version.