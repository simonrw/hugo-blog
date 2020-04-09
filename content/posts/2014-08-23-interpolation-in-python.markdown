---
title: Interpolation in Python
date: 2014-08-23 10:55:26
comments: true
Tags:
- python
- scipy
Category: tech
---

For interpolation in python, scipy includes the interpolateackage containing (amongst other things) interp1d for simple interpolation.

The function does not however perform extrapolation; if the interpolator is asked for a value outside the original range it will raise an exception. To get around this, the interpolator contains a `.x` parameter which contains the original `x` values used to construct itself. A boolean index can then be used to reject inputoints which fall outside of this range:

```python
# Create an interpolator object from the training dataset
interp = scipy.interpolate.interp1d(data_x, data_y)

# Boolean index array for data points falling within the 
# training dataset range
ind = (new_x > interp.x.min()) & (new_x < interp.x.max())

# Finally create the new interpolated data, remembering
# to apply the index to the x data also
interpolated_x, interpolated_y = new_x[ind], interp(new_x[ind])
```

