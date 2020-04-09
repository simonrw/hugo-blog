---
layout: post
title: vim crashing with ruby on OSX
date: 2013-09-05 15:44:00
comments: true
Tags:
- osx
- vim
- ruby
Category: tech
---

I started having trouble with vim crashing for me. I'm on OSX with homebrew and rbenv, and it crashed when editing Ruby files. I would get a segmentation fault. 

After some googling which was not particularly fruitful, the solution I came up with was:

* switch to the system ruby `rbenv shell system`
* uninstall vim completely with homebrew `brew uninstall vim`
* reinstall vim from head `brew install --HEAD --override-system-vi vim`

This seems to have fixed the problem, though I do not know if it was switching to the system ruby or installing from head. This may require extra study which I do not have time to do now.
