---
Title: "The :Explore command in vim"
Date: 2013-05-20
Category: tech
keywords:
- vim
layout: post
comments: true
---



In `vim` a useful command is the `:Explore`, which opens the netrw browser at the location of the current file. This is especially handy for editing files over `ssh`, but is handy for those who don't like the file tree type plugins.

The command can be shortened to `:E` which is fantastic, quickly browse the file system inside vim.

<!--more-->

![editing]({{ site.url }}/assets/images/editing-file.png)

This shows this current file in my editor. By pressing `:E` we get


![local]({{ site.url }}/assets/images/local-contents.png)

and by selecting `..` twice I get the top level of this blog

![blog]({{ site.url }}/assets/images/blog-contents.png)

The usual vim search commands are available for finding files so I encourage you to check it out.


