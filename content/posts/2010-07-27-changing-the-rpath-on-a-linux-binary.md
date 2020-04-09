---
Title: Changing the rpath on a linux binary
Date: 2010-07-27 10:15:00
category: tech
keywords: linux, c
layout: post
comments: true
---



[PatchELF](https://nixos.org/patchelf.html ) is a program that allows the patching of the rpath of a
linux binary to be whatever you want, even the printing of it.

<!--more-->

**Short explanation:**

Shared libraries are used in an operating system to allow the easy
updating of code and patch fixing to a large group of programs. With
static libraries, a patchfix to a library common to many programs would
force the recompilation of those programs. A fix to a shared library
could go on behind the scenes without these other programs even
realising (in the best case!).

To accomodate for this functionality, shared libraries must be located
during runtime. There are three ways to do this (in order):

1.  System default paths
2.  The binaries `rpath`
3.  The `LD_LIBRARY_PATH` enviroment variable


Libraries found in stage 3 will override libraries found in stage 2 and
the same for 2-\>1. Because of this overriding, the `LD_LIBRARY_PATH`
method is very dangerous, as the user can accidentally change this and
cause many programs to break.

As far as the sysadmin is concerned, the best way to install new
software is to update the local database which contains the system
default paths, but for the developer who wishes to compile their own
programs, option 2 is the best.

See my [previous post](/posts/2010-07-08-linking-on-linux/) to see how to do this at compile time, but say
while developing a piece of software, you want to see what these paths
are or change them. This is where `PatchELF` comes in handy. It allows
the printing of the `rpath` variable or setting it to a completely new
location (eg. for testing against a custom version of the system
libraries).  It even allows the changing of the system default path
interpreter.

For more information see [this very informative post](https://www.eyrie.org/~eagle/notes/rpath.html ).
