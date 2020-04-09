---
date: 2011-06-14 15:16:27
title: Plotting unevenly gridded contour plots
keywords: python, visualisation
category: tech
layout: post
comments: true
---



Quite often at work I have to generate colour maps of certain things, which are generally not sampled evenly in coordinate space. I am also a huge fan of Python, so I thought to myself: can I combine these things? Well until now I didnt think you could. Matplotlibs contour plots require evenly spaced x and y points with z points to match. This is until I found a way

Firstly generate an evenly spaced grid of your x and y data. The simplest way to do this is for data x and y:

``` python
xi = numpy.linspace(x.min(), x.max(), 100)
yi = numpy.linspace(y.min(), y.max(), 100)
```

The function that does the magic work is matplotlib.mlab.griddata and is based on Matlabs griddata function. It takes 5 parameters:

the raw x and y values
the set of colour data (z)
the evenly spaced x and y values (xi and yi)

``` python
zi = matplotlib.mlab.griddata(x, y, z, xi, yi)
```

This can now be used with plotting functions like contourf, eg.

``` python
import matplotlib.pyplot as plt
plt.contourf(xi, yi, zi, 25)
```
