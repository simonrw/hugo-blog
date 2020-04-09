---
layout: post
title: cello voice
date: 2013-08-24 12:05:00
comments: true
Tags:
- OSX
Category: tech
---

So Mac OSX has the `say` command, which reads out some text in the computers voice synthesis functionality. This is old news.

The new hotness is switching into the "cello" voice with `say -v cello`. I's lyrical and beautiful, though reading out file names is entertaining:

```
say -v cello source/_posts/2013-08-24-cello-voice.markdown
```


This was brought to my attention through the `guard-shell` gem [README](https://github.com/guard/guard-shell#saying-the-name-of-the-file-you-changed-and-displaying-a-notification).
