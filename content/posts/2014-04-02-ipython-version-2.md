---
layout: post
title: IPython version 2.0
date: 2014-04-02 16:32:38
comments: true
Tags:
- python
Category: tech
---

So IPython has updated to version 2.0. The full changelog [can be found here](https://ipython.org/ipython-doc/dev/whatsnew/version2.0.html) and to summarise the key points:

* interactive widgets for the notebook
* directory navigation in the notebook dashboard
* persistent URLs for notebooks
* a new modal user interface in the notebook
* a security model for notebooks

I want to discuss a few of these and my thoughts on how they make IPython notebook finally an incredibly powerful tool for research, more so than before, and how I may be finally switching from simple scripts to using the notebook full time (with a major caveat!)

## Directory navigation and persistent URLs

This feature enables navigation to seemingly an arbitrary location relative to the initial directory that `ipython notebook` was called in. I suppose this prevents access to sensitive locations or locations that the user wants to keep out of the search path. For example all of my work is under a root directory of `~/work` so if I start a notebook here then my other directories further up are not visible.

The following screenshot gives the example on the IPython homepage, and shows the navigable locations.

{{< figure src="/posts/images/treeview.png" title="IPython's treeview" >}}

With this a single server can manage all of the notebooks on a single machine. I was hoping for this functionality rather than either 

* having a single directory with all notebooks in, or
* running a separate server instance for each project.

Neither of these options were ideal and it really was preventing me from switching full time from individual scripts to IPython notebook, but with this change my ideal working method is now available.

## Modal interface

As a vim user I did not enjoy having to use emacs-ey keyboard shortcuts (`c-m s` to split a pane, for example). The modal editing allows most of the prefixed functionality to be put into "command mode" like moving to a different input box, and keeping editing commands concise.

## The final caveats

<!-- vim keybindings -->
Of course I've yet to find a vim emulator that I'm happy with. [Neovim](https://neovim.org/) promises to add an embeddable vim engine, but quite frankly I'm not really convinced that anything will allow me to get the vim feeling in another program.

This is of course not a failing in IPython notebook specifically, but of anything that isn't vim really...


<!-- nicer format so git diffs are better -->

One final thought, not about the new changes but about IPython notebook files in general. Yes the save format is renamed json and so is human readable and can be diffed, but has anyone really usefully diffed a notebook? It's basically impossible. Perhaps I've just not tried hard enough, and now with IPython 2.0 I may become more accustomed to diffing IPython notebook files and realise the folly of my ways.
