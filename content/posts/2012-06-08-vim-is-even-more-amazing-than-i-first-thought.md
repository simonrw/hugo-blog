---
date: 2012-06-08 11:09:23
title: Vim is even more amazing than I first thought!
Category: tech
tags:
- vim
layout: post
comments: true
---



Just today thanks to this answer on SO, I realised that vim can automatically pipe text through a shell command.
Just think about that for a second
For a simple example you can write python code into a text file, and execute it which replaces the code with the result.

```python
for i in xrange(10):
    print i
```

Selecting this text with in visual mode (you should all know how to do this) and typing

```
!python
```

which will get replaced with

```
','!python
```

since youre in visual mode. This will replace the code written above with the numbers from 0 to 9.
This could even then be piped into eg. `!sort -r` to reverse the list.
A use I've come up with is to pipe code into grep for data file formatting.
