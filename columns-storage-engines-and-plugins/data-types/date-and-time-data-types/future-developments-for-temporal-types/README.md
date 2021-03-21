# Future developments for temporal types

My current project is a forecasting application with dates going out to 2638 max at the moment. That's not a problem with the max date value for the MariaDB date type 9999-12-31, but it's a quarter of the way there.

I recently had an issue that brought me here - I was trying to pass a value from a Java LocalDate with the value LocalDate.MAX (+999999999-12-31).

I can imagine other users, e.g. astrophysicists, mathematicians, geologists, historians etc would need dates outside the 1000 - 9999 range.

Is there anything in the pipeline?

Is there an idiom for roll-your-own date types that store the year in an unsigned long, and the month and day in tiny ints, or similar?

## Answer
            <span class="answer_comment">Answered by 
<span class="user" id="user-1368">
[Ian Gilfillan](/kb/user/id/1368)
</span> in [this comment](#comment_3892).</span>

There's nothing in the pipeline currently as far as I'm aware. For dealing with dates beyond, integers is probably the best way to go, although of course date calculations are then not possible using the built-in functions.