---
layout: post
title: Using guard with foreman
date: 2013-07-29 11:09:00
comments: true
tags:
- ruby
- guard
- foreman
- latex
Category: tech
---

I've been playing with [Ruby](https://www.ruby-lang.org/) quite a bit recently and I like the way a custom-looking DSL can be created from the same source language.

This means I've converted a lot of my tools (e.g. Makefiles) into Rakefiles and I've just found [foreman](https://ddollar.github.io/foreman/) which can manage multiple subprocesses and combine their output into a nice looking coloured console result. This is typically used to manage web processes (server, task runner, development database etc.) and is required by [heroku](https://heroku.com).

I was using this technique with a simple `Procfile` for managing my thesis. I have two tasks running: [latexmk](https://users.phys.psu.edu/~collins/software/latexmk-jcc/) which manages the latex compiling, and [guard](https://guardgem.org/) which manages the python plotting.

I added these to the `Procfile` as such

``` ruby
latex: latexmk -pvc --pdf Thesis
plotting: bundle exec guard
```

This would cause both processes to hang, which I couldn't explain. I then thought that guard is interactive: it has an interface for managing specific tasks or getting info about the processes running. I searched through the guard help to find the `--no-interactions` flag and this solved my problem. My new complete Procfile is now

``` ruby
latex: latexmk -pvc --pdf Thesis
plotting: bundle exec guard --no-interactions
```
