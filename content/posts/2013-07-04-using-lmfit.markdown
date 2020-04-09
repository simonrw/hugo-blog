---
layout: post
title: Function fitting with lmfit
date: 2013-07-04 18:49:00
comments: true
Category: tech
tags:
- science
- python
---

[Scipy](https://www.scipy.org/) contains functions for fitting equations with [Python](https://www.python.org), in its `scipy.optimize` module. The two main ones I've used in the past are `leastsq` and `curve_fit`, which in itself is a convenience wrapper around `leastsq`.

## `curve_fit`

For this operation you require three (four) things:

* a function to fit of form `f(x, *params)`
* `x` data
* `y` data
* Optionally error data

You can also supply an initial guess with the `p0` argument.

For example a simple linear equation

```python
def f(x, a, b):
    '''
    Fit a linear function y = a * x + b
    '''
    return a * x  + b
```

# Load the data to fit into x, y and e arrays

```python
p0 = [1., 1.]
popt, pcov = curve_fit(f, x, y, sigma=e, p0=p0)
```

In this example `popt` contains the "optimal" results, and `pcov` the covariance array after fitting.

The problem
-----------

This method of working is very powerful but you cannot place limits on the extent of the input fitting parameters, and you are locked into using the `leastsq` underlying function with this nice interface. For example the function

```latex
$$
f(x) = x^a
$$
```

will explode and the fitting routine will complain when asked to fit around `a = 0` when `x = 0`.

`lmfit` provides this functionality in a convenient object-oriented interface. The function to fit is phrased a little differently but the functionality is the same.

The easiest way is to write out the desired function as with `curve_fit` and include a "residuals" wrapper to calculate the normalised (if errors are given) distance away from the model

```latex
$$
R = \frac{y - m}{\sigma}
$$
```

where `$R$` are the residuals, `$y$` are the observed y values, `$m$` the values as calculated by the function that's being fitted, and `$\sigma$` the uncertainties. These residuals tested so that the square of the value above is minimised. The functional form of the residuals function is a little different, and parameters must be accessed with a dictionary lookup and the attribute `value`.

```python
import lmfit

def resids(params, x, y, e):
    '''
    Given the function f outlined above, return the normalised residuals
    '''
    a = params['a'].value
    b = params['b'].value

    model = f(x, a, b)
    return (y - model) / e

# generate the input parameters
params = lmfit.Parameters()
params.add('a', value=1., min=0, max=10)
params.add('b', value=1., min=0, max=10)

out = lmfit.minimize(resids, params, args=(x, y, e))
```

The above code alters the `params` object in place so the best fit parameters are given with

```python
print "Best fit a: {}".format(params['a'].value)
print "Best fit b: {}".format(params['b'].value)
```

The uncertainties can be accessed with the `stderr` parameter. A nice report can be printed with `lmfit.report_fit(params)`.

Crucially the initial values for the parameter guesses as well as bounds and (something I've not explored much) functional relationships between them. This is a simple way of applying some basic Bayesian priors to the parameter assumptions.
