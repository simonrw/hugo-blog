---
date: 2013-02-05 11:38:00
title: Embedding Python in LaTeX
Category: tech
tags:
- python
- LaTeX
layout: post
comments: true
---



Combining Python with LaTeX is a powerful combination. It allows for arbitrary
code to be executed which either gives the results of expressions, or can be
used to embed programatically certain things e.g. the paths of files or images.

By downloading python.sty and including it in a usepackage block, python code
can be run.

I was having trouble getting this to work as it seems I was using an invalid
version of python.sty. The version linked above works as of the date of this
post

This must be compiled with latex/pdflatex and the -shell-escape option. Probably
any latex compiler which supports this flag will do so probably luatex will
support this, but I dont really see the point in embedding python in luatex
compatible latex!

I believe any python module thats available to the system python installation
can be used, so the following will print the current directory the latex was
compiled in

An optional python script can be included with the compilation. A common usage
is to import the modules required for the whole project, or to get some global
variables


