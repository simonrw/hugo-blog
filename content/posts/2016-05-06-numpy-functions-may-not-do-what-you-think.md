---
title: Numpy functions may not do what you think
date: 2016-05-06 09:42:56
tags:
- python
- numpy
---

Numpy has the ability to mask arrays and ignore their values for certain computations, called "masked arrays". They contain a `.mask` attribute which is a boolean array, `True` where the value should be masked and `False` otherwise.

Numpy also comes with a suite of functions which can handle this masking naturally. Typically for a function in the `np.` namespace, there is a masked-array-aware version under the `np.ma.` namespace:

```
np.median  => np.ma.median
np.average => np.ma.average
```

A crucial thing to remember however is that **standard `numpy` functions ignore the mask for a masked array**.

This caught me out when sigma clipping values using `astropy.stats.sigma_clip` - which masks out values outside the sigma range. To ignore the sigma clipped values I should have used `np.ma.median` instead of `np.median`.
