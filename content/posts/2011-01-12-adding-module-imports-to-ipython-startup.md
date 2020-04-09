---
Title: Adding module imports to iPython startup
Date: 2011-01-12 10:19:00
category: tech
keywords:
- python
- ipython
---



I'm constantly opening an iPython interpreter and having to import my
common modules (for me [pyfits](https://www.stsci.edu/resources/software_hardware/pyfits ) mostly).

The easiest way to import modules on iPython startup is to look in your


``` python
~/.ipython/ipy_user_conf.py
```


file which is a nice easy way to add python code into your ipython
startup (as opposed to the ipythonrc file which is about colours etc.).

In the main function, just add a line such as


``` python
ip.ex("import pyfits")
```


This will allow custom code to be run at startup. In this case, iPython
loads the pyfits module.

You could add such things as


``` python
ip.ex("from pylab import *; x = arange(100); y = sin(x)")
```


for example if you were working with sine waves a lot.
