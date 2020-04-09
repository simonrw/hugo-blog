---
title: Making windows feel more like Linux
Authors: Simon Walker
date: 2018-07-03 20:22:23
category: tech
tags:
- windows
- linux
---

*I haven't posted for a while, and this is not going to be a very useful
post, you have been warned...*

At work we use Windows. I vowed when I left my last job that I'd never
go to work somewhere that uses Windows.

I am very much still not a fan, especially since I have not got admin
rights and we are using Windows 8 - one of the most hated versions of
Windows since ME.

I have been using Linux personally and professionaly for a decade now, and am so optimised to live on the command line that Windows is
genuinely slowing me down at work.

There are a few programs that help with this. They do not recreate my
life on the command line as before, but they help with general
behaviours.

# Mouse-dragging of windows

Most Linux desktop environments support dragging a window by holding Alt
and dragging with the left mouse button. Additionally, some of them
support resizing windows by holding alt and dragging with the right
mouse button.

[AltDrag](https://stefansundin.github.io/altdrag/) is the program that
provides this functionality for Windows.

# Scrolling the inactive window

In Linux, scrolling the mouse wheel over an inactive window scrolls the
content. This does not happen on Windows, at least Windows 8.

Again, [AltDrag](https://stefansundin.github.io/altdrag/) provides this
functionality and is crucial when for example, writing some notes on a
webpage and scrolling the page without selecting the web browser.

# Task launcher

The Windows 8 search functionality for programs is abysmal. Compared to
the lightning pace of life when on the command line, the Windows 8
search box performance is terrible.

If it's accuracy were perfect, I'd try to put up with the terrible
performance, but it also is not.

I switched to [Executor](http://executor.dk/) which I've bound to the
Spotlight-default Alt+Space. It starts instantly, looks great, and finds
programs very well.

# Vimium

This is actually a feature that I use in Linux as well. Given I use
[Vim](https://www.vim.org) as my text editor, I am so acustomed to the
key bindings and modal editing in general, I strive to use Vim
keybindings everywhere.

I have tried many Vim shortcut plugins for my web browser over the
years, and [Vimium](https://vimium.github.io/) is the best I've found.
Since Firefox switched to WebExtensions, this plugin has worked with
Firefox reinforcing my opinion that Firefox is my favourite browser.

# Keyboard shortcuts

Part of living life on the command line is never using the mouse.
Windows has some general keyboard shortcuts that can help, and some
programs have shortcuts that help.

# Text editors

Luckily for me, my text editor of choice [Vim](https://www.vim.org) is
supported on Windows, as is [Neovim](https://neovim.io/).

I am also having good success using [Visual Studio
Code](https://code.visualstudio.com/) - with [Vim
keybindings](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim),
including true Neovim integration. The plugin ecosystem is excellent.

# Portable apps

This is not really a program that makes Windows feel more like Linux,
but it does help getting around the lack of admin rights.

[Portable Apps](https://portableapps.com/) are a way to run quote a lot
of Windows programs without "proper" installation. I presume this means
they do not need to set registry keys[^1] etc. This way I can have my
text editor, a decent web browser and a few other programs.

[^1]: don't get me started on a global binary method of storing
  configuration, rather than simple plain-text scripts in standard
  locations...
