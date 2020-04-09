---
title: Separate IPython profiles for interactive use
date: Sun Jan 18 20:33:57 GMT 2015
tags:
- python
- visualisation
- ipython
---

I used to have two simple shell aliases for `IPython`:

``` sh
alias ipy=ipython
alias pylab='ipython --pylab'
```

These were separated for a couple of reasons:

The `pylab` mode of `IPython` was deprecated, for good reason. It "infects" the global namespace with all `matplotlib` and `numpy` functions. It breaks two entries in the famous "Zen of Python":

* Explicit is better than implicit.
* Namespaces are one honking great idea -- let's do more of those!

From a practicality perspective it also is much slower to start up. This is annoying when I quickly want to plot something.

I then remembered the `IPython` profile functionality. A separate profile, and hence separate configuration and startup procedure for each profile. I customized my configuration for each profile in basically the same way: I turned off the startup banner - I've seen it enough times now! - and configured the log format. In the "pylab" form I set `c.InteractiveShellApp.pylab = 'auto'` to enable the interactive plotting, with a backend chosen for me, and set `c.TerminalInteractiveShell.autocall = 2`. This last one may be a bit controversial, so I'll explain a bit further.

### `autocall`

This setting (sometimes) removes the need for parentheses when running python code. For example:

``` python
def stuff(value):
    print(value + 5)

stuff(5)   # Normal python
stuff 5    # Valid with autocall = 1 or 2, see below

def more_stuff():
    print('Hi there')

more_stuff()   # Normal python
more_stuff     # Exception in all but autocall = 2
```

This setting has three options: `0` to disable the feature, `1` to enable it in a "smart" sense, and 2 where it's enabled always. The main difference between `1` and `2` is when a "bare" function is called, e.g. `more_stuff` in the example above, without parentheses. With `autocall = 2` the function is *called* rather than merely returned.

## Starting up with `seaborn`

I am a fan of the [seaborn](https://stanford.edu/~mwaskom/software/seaborn/) visualisation package, for the styles and extra features. In the `pylab` profile I set this up to automatically import seaborn and silently fail if it's not found. This code can be found [in this file](https://github.com/mindriot101/dotfiles/blob/master/ipython/ipython/profile_pylab/startup/00-with-seaborn.py). I'll explain it here.

The code:

```
from __future__ import print_function

try:
    print('Loading seaborn...', end=' ')
    import seaborn as sns
except ImportError:
    print('Cannot load seaborn')
else:
    print('Done')
```

I start by importing `print_function` from `__future__` to ensure python 2/3 compatibility. Next in a `try/except` block I try importing `seaborn`, under the `sns` prefix which I'm accustomed to. Should an import error occur (such as I have not got the package installed in the current `conda` environment) I tell the user. The print functions are called such with `... end='')` to ensure the status message is all on one line, purely for aesthetics.

Current configuration
---------------------

My current configuration can be read for

* "Normal" mode: [https://github.com/mindriot101/dotfiles/blob/master/ipython/ipython/profile_default/ipython_config.py](https://github.com/mindriot101/dotfiles/blob/master/ipython/ipython/profile_default/ipython_config.py)
* "Pylab" mode: [https://github.com/mindriot101/dotfiles/blob/master/ipython/ipython/profile_pylab/ipython_config.py](https://github.com/mindriot101/dotfiles/blob/master/ipython/ipython/profile_pylab/ipython_config.py)
