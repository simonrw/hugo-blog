---
Title: Allowing the use of clang in qmake
Date: 2010-07-01 14:54:00
---



Clang is an attempt at creating a gcc replacement front end for \*nix
based systems with static code analysis and more helpful error messages
etc.

To allow the use of the clang compiler, the easiest way is to create a 
new *mkspec*, and create a Makefile with


``` bash
qmake -spec linux-clang
```


See the link at the end of this post for the original credit, and a
sample *mkspec* file.

[Credit here](https://www.freehackers.org/thomas/2010/01/10/playing-with-clang-and-qt/)
