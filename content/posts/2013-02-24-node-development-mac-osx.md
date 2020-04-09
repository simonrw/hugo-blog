---
Title: (node) development on OSX
Date: 2013-02-24
Category: tech
keywords:
- node
- OSX
- productivity
---



I love my Macbook Pro, for development and general computing use. Before I
purchased it I was a die hard linux user (apart from gaming which is still
dominated by windows unfortunately.) My work involves linux[^my-job] and I am a
committed [vim](https://vim.org) user. 

My work colleagues tend to laugh when I say that I want to use my mouse as
little as possible, and that I am quite experienced in command line use. It
pains me every time I see my office mates reach over to the mouse to click the
*save* button.

This workflow does not translate too well to the Mac; as far as I understand it
Apple[^nextstep] were one of the first companies to introduce guis for their
operating systems.

<!--more-->

On the other hand I love the user interface, operating system and window/font
rendering systems. It's quality hardware and a fantastic (IMO) keyboard,
coupled with excellent software and makes me feel really good using it. It's
become my main operating system at work now since I code almost exclusively on
my macbook. Occasionally I have to SSH into the work computers to access data
but usually this is from my laptop.

I also like automation.

During (node) development I much prefer editing code in my editor and either
seeing it instantly in my browser, or just reloading it myself. For this and
other development, I use a few tools:

* [Homebrew](https://brew.sh) for package installation
* [Better snap tool](https://blog.boastr.net/?page_id=2342) for tiling windows
  side by side
* [Macvim](https://code.google.com/p/macvim/) for editing, `brew install macvim`
* [iTerm2](https://www.iterm2.com)
* [when-changed.py](https://github.com/joh/when-changed) for
  compiling/running/whatever when files have changed

I'll discuss these tools in turn.

## Homebrew

Linux has it's share of problems, which should be classed as more *challenges*
than problems as it's fun I assure you. On the other hand it's package
management system is top notch. The Mac OSX equivalent is Homebrew and is
superbly managed with packages on [github](https://github.com). This takes the
difficulty out of installing a lot of packages, but is still missing some nice
features.

The nice thing about Linux's package management is that the packages are
*binary* packages, so no compilation is required. They are compiled by someone
with extensive knowledge of the package and operating system - usually the
package maintainer. Packages on Mac OSX still have to be compiled from source
so can take a long time.

## Better snap tool

Having multiple windows open simultaneously is a huge timesaver as you can see
the contents simultaneously. Having multiple monitors is a fantastic but
expensive option, but if on a single window you could resize the windows
yourself but this can be time consuming if it needs to be done many times.

This tool allows some keyboard shortcuts to resize the windows to take up half
the screen each. There exists a keyboard shortcut for restoring the windows
back to their original position as well.


![Screenshot]({{ site.url }}/assets/images/screencap.png)


The reason I chose this tool over other similar tools is that it allows for a
nice border around windows which I could not find with other tools. It's a
small point but I like to keep the look of OSX as much as possible and I feel
like non-fullscreen windows fits this bill.

## Macvim 

I mentioned that vim is my editor of choice, and macvim is the epitome of this
editor for OSX.

## iTerm2

This is a great replacement for the Terminal.app that ships with OSX but
otherwise is a simple terminal emulator. It has nice support for profiles (e.g.
setting up an ssh link from a few keystrokes). 


## when-changed.py

This script watches for filesystem changes, and runs a simple command when
something is different.

The command used for writing this blog post is as follows:

``` bash
when-changed.py $(find content -name '*.md') -c 'fab build'
```

Any time I save the post, by build system ([fabric](https://fabfile.org))
recompiles the post so I can just reload the browser to see the changes.

### Application to node

[node](https://nodejs.org) actually comes with a tool similar to
`when-changed.py`, it's called `supervisor`, and can be installed globally with
npm: `npm install --global supervisor`. It can reload the server file with any
changes. I can edit it in one pane with macvim, and view the changes in the
browser by just reloading the page.

*Bliss*


[^my-job]: I am currently working towards a PhD in astronomy, and very little
astronomy is done on Windows.

[^nextstep]: NeXTSTEP I believe
