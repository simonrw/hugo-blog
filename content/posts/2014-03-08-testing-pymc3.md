---
layout: post
title: Testing PyMC3
date: 2014-03-08 13:14:42
comments: true
Tags:
- python
Category: tech
---

I was trying to play with PyMC3 and as per usual with code under heavy development the tutorials were out of date, and the code wouldn't run. When I say "out of date" in fact the code ran but no valid numbers were produced. The API seemed to be consistent though.

I managed to get the [tutorial](https://nbviewer.ipython.org/github/pymc-devs/pymc/blob/master/pymc/examples/tutorial.ipynb) to run by installing the following:

    Theano==0.6.0
    pymc==3.0
    scipy==0.13.3

PyMC3 was installed from git from the `pymc3` branch as follows:

``` python
pip install git+https://github.com/pymc-devs/pymc@pymc3
```

I had some trouble installing the `pymc3` branch without installing the specific version of Theano shown.
