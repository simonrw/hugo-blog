---
Title: Installing development version of nginx with Homebrew
Date: 2013-02-25
Category: tech
keywords:
- homebrew
- OSX
- nginx
layout: post
comments: true
---



I have been trying to install nginx v1.3.x on my Macbook Pro for a while now to test the native [websockets support](https://nginx.com/news/nginx-websockets.html) but I could not find a way to install version 1.3.

<!--more-->

I had a look with `brew info nginx` and found a clue: a devel flag:


``` ruby
require 'formula'

class Nginx < Formula
    # Other code

    devel do
    url 'https://nginx.org/download/nginx-1.3.13.tar.gz'
    sha1 'b09b1c35b2b741292d41db1caa3b8a4123805a4c'
    end

    # Other code
end
```

I should really have found this earlier or tried to look harder, but it turns out homebrew has a `--devel` flag for the install command. This defines the `devel` variable and is caught by the if block above.

So simple really, but I googled to no avail, so hopefully this post will come up for anyone else who has this problem.
