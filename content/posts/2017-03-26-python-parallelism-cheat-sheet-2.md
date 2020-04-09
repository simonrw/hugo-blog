---
title: Python parallelism cheat sheet (part 2)
Authors: Simon Walker
date: 2017-03-26 20:31:55
category: tech
tags:
- python
---

_This blog post is the second in a series I am writing, covering methods of simple parallelism. The following posts cover more convenient methods, as well as some things that should be considered._

## Parallelism methods

* [Basics and introduction](/blog/2017/01/10/python-parallelism-cheat-sheet/)
* [Function objects](/blog/2017/03/26/python-parallelism-cheat-sheet-part-2/)

_If I've skipped your favourite method of parallelism, feel free to [tweet me](https://twitter.com/srwalker101) or [add a comment on the tracking issue](https://github.com/mindriot101/mindriot101.github.io/issues/2) informing me._

---

This method was brought to my attention by [Tom Marsh][1], and is a nice alternative to using [`functools.partial`][2].

In Python, a custom class instance can be treated like a function (technically a _callable_) if it has a `__call__` method:

```python
class Foo(object):
    def __call__(self):
        return 'Hello world!'

foo = Foo()
print(foo())  # Hello world!
```

We can take advantage of this for parallelism reasons, by replacing the construction of the partial function with a custom class, which stores any constant state as instance variables, and defines a `__call__` method to perform the work.

To recap, the problem we are trying to parallelise is:

```python
avalues = range(20)
bvalues = range(100, 120)
const = 100

results = []
for a, b in zip(avalues, bvalues):
    # pretend this computation is much more expensive
    results.append((a + b) * const)
print(results)
```

The parallelised version with [`functools.partial`][2] looked like:

```python
from functools import partial
from multiprocessing import Pool  # 1


def worker_fn(changing_stuff, const_value):
    '''A function that takes as it's first parameter
    the values that change for each loop iteration, and
    the remaining parameters do not change with each
    loop iteration
    '''
    a, b = changing_stuff
    return (a + b) * const_value

avalues = range(20)
bvalues = range(100, 120)
const = 100

fn = partial(worker_fn, const_value=const)

# this must be an `iterable`, so a list or generator
zipped_args = zip(avalues, bvalues)  # 2

# Python 3
with Pool() as pool:  # 3
    results = pool.map(fn, zipped_args)  # 4

# or if you're stuck with Python 2:
pool = Pool()
results = pool.map(fn, zipped_args)  # 4
```

Instead of creating a partial function, we define a class which stores constant values as instance properties, and defines `__call__`:


```python
from functools import partial
from multiprocessing import Pool  # 1


class Worker(object):
    ''' Function object, which wraps any constant state with the class,
    along with implementing a method to call with the varying argument
    values.
    '''
    def __init__(self, const_value):
        self.const_value = const_value

    def __call__(self, changing_stuff):
        a, b = changing_stuff
        return (a + b) * self.const_value

avalues = range(20)
bvalues = range(100, 120)
const = 100

fn = Worker(const_value=const)

# this must be an `iterable`, so a list or generator
zipped_args = zip(avalues, bvalues)  # 2

# Python 3
with Pool() as pool:  # 3
    results = pool.map(fn, zipped_args)  # 4

# or if you're stuck with Python 2:
pool = Pool()
results = pool.map(fn, zipped_args)  # 4
```

## Wrapup

Some things to note about this approach:

* technically implementing `__call__` is not necessary - due to Python's attribute access any function can be used:

```python
class Worker(object):
    def __init__(self, const_value):
        self.const_value = const_value

    def foo(self, changing_stuff):
        # implentation

# ...

worker = Worker(const_value=const)
with Pool() as pool:
    results = pool.map(worker.foo, zipped_args)
```

* this solution may cause issues with concurrency, which the `functools.partial` version does not.

Each process has access to the `Worker` instance, and due to lack of mutability controls in Python, can change anything on the instance. Should the `__call__` method change any state on the `Worker` instance then all hell breaks loose. With the `functools.partial` version, the function contents cannot change the value of the arguments passed so each function invocation has its own copy. This leads into

* the function objects _may_ be more (memory) efficient

I do not know for sure about this one, but as `functools.partial` objects copy the data across whereas function objects have the information stored once on the instance. This may mean that each _process_ has a copy of the constant data with function objects, instead of each _function invocation_.

Alternatively Python may be more clever than this, and the information may be accessed more efficiently 🤷

[1]: https://www2.warwick.ac.uk/fac/sci/physics/research/astro/people/marsh/
[2]: https://docs.python.org/3/library/functools.html#functools.partial
