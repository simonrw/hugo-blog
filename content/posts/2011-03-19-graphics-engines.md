---
date: 2011-03-19 12:00:08
title: Graphics engines
category: tech
keywords: gamedev
layout: post
comments: true
---



I've been looking into graphics engines recently, though this can only be a part time hobby for me since I should be doing my PhD work.
I've mostly been looking at two in particular:

* Ogre
* Irrlicht

Both of these seem to have some pros and cons, which I will be detailing in a second. I cannot decide which one I prefer though so maybe you can help me out? Comments are welcome.

## Ogre

Lets start with this one. Perhaps the more fully featured engine, and used in
some pretty famous games, it comes out of the box with some nice shader support
and a great tutorial which is easy to follow and gets you creating a scene in no
time at all.  This is one of my main problems though: the tutorial really hides
all the workings of what actually goes on in the engine from the tutorial
follower as its all contained in a TutorialApplication class which handles
things like setting up a camera, and rendering the settings box and gui.  This
is very nice to get a quick application going but doesnt really teach you
anything about the API. I suppose this might be explained further in the
tutorial but I never followed it that far (probably shouldnt admit that but
never mind!).

## Irrlicht
Had never heard of this one before I did some googling. It is less well known as
far as I can tell, but still used in some highly commended games.
The nicest difference is the tutorial exposes all the code to the user, which
can make the initial tutorials a little lacking in functionality (eg. no fps
mouse controls) but helps you understand much more.

## Comparison

Some differences in the use of the two engines are:

* Transformations - Ogre uses quaternions, a rotation around a vector, whereas
Irrlicht uses rotation matrices which are much easier to comprehend
* Utility classes - Irrlicht has such things as `vector3class T` which call a
templated function for `class T`. Ogre uses a built in (though it may be a
`typedef`) `Vector` class.

I am planning on adding to this post as I find more differences and this is only
a brief first impressions post, and I still cannot decide which I prefer!

