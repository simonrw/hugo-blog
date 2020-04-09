---
date: 2013-02-08 11:39:10
title: Nested gitignore files
Category: tech
tags:
- git
---



Today I found out that .gitignore files can be nested.

For example the directory tree

```
a
a/b
.gitignore
a/b/.gitignore
```

can have files in `b` ignored by `a/b/.gitignore`.

This helps split the responsibility of ignoring files by topic and really helps organisation!
