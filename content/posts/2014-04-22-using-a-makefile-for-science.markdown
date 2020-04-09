---
layout: post
title: Using a Makefile for science!
date: 2014-04-23 10:27:49
comments: true
Tags:
- make
- code
- science
Category: Tech
---

The declarative style of a Makefile (or substitute your own language-of-choice's implementation of \*akefiles) lends itself well to scientific processes, where a reproducible method is crucial. I recently found this out when analysing some data, which consisted of the following:

1. compile the source code of the programs I was using
2. use these programs on the same data
3. plot some summary plots
4. show the plots

With the declarative style of Makefiles I could rely on the latest results no matter what changed in the analysis path, for example the plot scripts used in step 3. were listed as a dependency of the step run to make the plots so when the script changed, the plots would be recreated. 

Another advantage is that step 2. took a long time, so the "update only when the dependency is out of date" style of makefiles really came in handy. I could change the (relatively quick) plotting scripts to my hearts content safe in the knowledge that the data I was using was up to date.

For scientific analysis, reproducibility is of prime concern, and should I come back to this project at a later date, the combination of a makefile plus the fact that the project was stored in a git repository makes the code easy to use (the readme just contains the line: `make`) and consistent.

A small tip that I did not know before starting out on this endeavour (and is specific to GNU make) is to use `$(MAKE)` whenever running make in a subdirectory. This enables parallel job flags (e.g. `-j 4`) to be passed down to subsequent make invocations.

For reference, I was getting the message:

```
warning: jobserver unavailable: using -j1.  Add `+' to parent make rule.
```

when calling subtasks with a simple `make`. Now with `$(MAKE)` the jobs run in parallel where possible.
