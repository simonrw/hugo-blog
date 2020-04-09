---
layout: post
title: "Failed loading plugin: copr"
date: 2014-06-16 16:09:35
comments: true
Tags:
- linux
- fedora
Category: Tech
---

If you're:

* running Fedora
* using dnf as a package manager
* get the following error: `Failed loading plugin: copr`

then listen up, your solution is at hand: install `python-requests` using yum:

```
sudo yum install -y python-requests
```

I have not tested if this will work installing with pip yet.
The linked bug report suggests that this will be registered as a dependency as of 2014-06-05 so this problem should go away.

Source: [this bug report](https://bugzilla.redhat.com/show_bug.cgi?id=1104088)


  
