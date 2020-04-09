---
Title: Stupid programming mistake
Date: 2013-02-27
---



So I was coding the other day and had some strange behaviour from one of my Python scripts. I was running two boolean checks which update a variable. Both should be `|=` types, but it turns out a `!` character looks incredibly similar (to my tired eyes at least) to the `|` character which of course is a completely different boolean operator.

* Before

![wrong]({{ site.url }}/assets/images/wrong.png)


* After

![correct]({{ site.url }}/assets/images/correct.png)

Always something new!
