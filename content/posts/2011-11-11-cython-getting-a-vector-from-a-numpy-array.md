---
date: 2011-11-11 12:05:21
title: "Cython: getting a vector from a numpy array"
Category: tech
keywords:
- python
---



 This is a problem that has been challenging me for a while: my c++ code uses vectors everywhere so how can I wrap these classes and functions into python easily? I've tried many many times with e.g.  swig or boost::python to no avail. 
That is until today…

<!--more-->

Thanks to the very kind people at Stack Overflow and this question in particular, I was able to create a wrapper function for Cython which does exactly what it says on the tin.The full code for the wrapper function is given here:

```python
from libcpp.vector cimport vector
cimport numpy as np

# Utility function to convert from numpy array to vector
cdef vector[double] arrayToVector(np.ndarray[np.double_t,ndim=1] array):
    cdef long size = array.size
    cdef vector[double] vec
    cdef long i
    for i in range(size):
        vec.push_back(array[i])

    return vec
```

I shall try and explain this now. Firstly the Cython vector library (libcpp.vector) contains a pre-wrapped vector class which is great but its incompatible with the numpy array class (hence why this is a problem!). Importing numpy (with cimport) at this stage is also required. 
The declaration is a little verbose for my liking (especially compared to python) but the argument is declared as a ndarray (the base class to array) with some templated parameters, using the square bracket notation in cython as opposed to the triangular bracket notation in c++. It shall hold double datatypes and it has only one dimension. The function also returns a double vector. It is safe to declare it with cdef as this will enable more optimisations and it wont need to be called directly from python anyway. 
I have yet to find a memcpy-type function in cython so I am then declaring the variables used (another Cython optimisation) and appending the array one value at a time into the created vector object. Finally I return the created vector ready to be used as an argument to an external c++ function.
The setup.py file is identical to wrapping any other c++ code and is documented well on the Cython website itself so I wont go into it here. 
Once I get more comfortable with the syntax I might try and write a templated version of the function, but for now I am content using the double version and writing another piece of code if I need eg. an integer function.
