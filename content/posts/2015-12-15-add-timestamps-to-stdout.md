---
title: Add timestamps to stdout
date: 2015-12-15 13:41:39
category: tech
---

I spent some time trying to get timestamps added to C++ printing, e.g .through `cout`.

I naive approach is to write a function `get_current_time()` and put it before all printing statements e.g.:

```cpp
cout << get_current_time() << "Message" << endl;
```

This requires changing all logging statements. Then my googling stumbled upon [this question](https://stackoverflow.com/questions/22118713/add-time-stamp-with-stdcout) which had an [elegant solution](https://stackoverflow.com/a/22119115/56711) incorporating a decorator object.

Further down the page however I came upon a much nicer solution that transcends languages and programs and can be applied to running shell commands. [The answer I ended up stealing and adding to my .zshrc](https://stackoverflow.com/a/22130756/56711) is as follows:

```sh
function add_timestamps() {
    awk '{print "["strftime()"]: "$0}'
}
```

This can be used after a program is run e.g. `a.out | add_timestamps` to add the current time before every line printed out by `a.out`.

Even `top` can be passed through this function, but with predictably nasty results!
