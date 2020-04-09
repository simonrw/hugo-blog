---
date: 2011-06-13 14:42:00
title: Installing IT++ on Mac OSX
keywords:
- OSX
- itpp
category: tech
layout: post
comments: true
---



IT++ (or itpp) is a library of Matlab-like functions. I was trying to install this as it contains a nice FastICA algorithm, but was having trouble compiling it on my laptop. I tried googling and found this, but it didn't work for me. 

My issue was that it was not linking to the ATLAS library, which I required for fast computation. IT++ installed without it, but I wanted these features, so after wading through many options, I managed to root out the problem.

I have ATLAS installed through MacPorts, which is all well and good, but the software from there is 64 bit whereas all compilers on the Mac are 32 bit. This caused the configure script to fail with the wrong things. Instead of not being able to find ATLAS because it could not find the functions in the libraries, the compiler was failing to compile the file due to fortran cannot find symbol in architecture x86_64 type errors. 

After using the `F77=gfortran-mp-4.4` which is the 64 bit macports fortran compiler, it managed to configure just fine. Just a warning: this is not the first time I've had strange cannot find symbol  errors even though I know Ive got the libraries installed. Almost all of my troubles are from fortran, reinforcing my hatred of the language. 
