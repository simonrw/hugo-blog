---
title: "How I Debug"
date: 2020-06-05T16:07:55+01:00
tags:
- python
- debugging
- pytest
summary: How I debug failing Python tests.
---

A discussion came up today about how to debug failing tests.

My strategy typically follows this path:

* look at the stacktrace and code and try to figure it out,
* insert print statements into the code to get key values
* use `pudb`

The first two of these are reasonably self-explanatory, but I wanted to go into
more detail about the last point, using `pudb`.

## Using `pudb` to debug failing tests

`pudb` is an improvement on top of the built in `pdb`. Given a file:

```python
# inside example.py

def foo():
    print("Inside foo")
    value = bar()
    print(value)

def bar():
    print("Inside bar")
    return 15

foo()
```

we can use `pdb`:

```
$ python -m pdb example.py
> /tmp/example.py(1)<module>()
-> def foo():
(Pdb) n
> /tmp/example.py(6)<module>()
-> def bar():
(Pdb) n
> /tmp/example.py(10)<module>()
-> foo()
(Pdb) s
--Call--
> /tmp/example.py(1)foo()
-> def foo():
(Pdb) s
> /tmp/example.py(2)foo()
-> print("Inside foo")
(Pdb) n
Inside foo
> /tmp/example.py(3)foo()
-> value = bar()
(Pdb) n
Inside bar
> /tmp/example.py(4)foo()
-> print(value)
(Pdb) p value
15
(Pdb)
```

Here I am stepping through the code and once I reach `value = bar()`, I can
print `value`.

Starting `pdb` from the command line doesn't work with tests. Instead, I insert
this snippet:

```python
import pdb; pdb.set_trace()
```

which opens a `pdb` session at the current line.

`pdb` is great, but it has one major drawback: the amount of information visible
on the screen. You cannot see your code, local variables, the current stack or
information about breakpoints you have set.  I said I used `pudb` instead of
`pdb`. What's `pudb`?

[`pudb` is a full-screen console debugger for Python](https://documen.tician.de/pudb/)

![Example image stolen from https://rayblick.gitbooks.io/my-python-scrapbook/basics/debugging/pudb_debugger.html](https://rayblick.gitbooks.io/my-python-scrapbook/basics/debugging/Screenshot%20from%202016-01-08%2019:28:36.png)
*Image shamelessly stolen from [this blog post](https://rayblick.gitbooks.io/my-python-scrapbook/basics/debugging/pudb_debugger.html)*

It opens a full-console debugger. The source code can be seen, breakpoints can
be set interactively on lines, local scope can be inspected as well as the
current stack position.

I am a full time vim user so I tend to use other terminal-based tools. This
debugger is perfect for those really tricky situations where interactivity is
required. A quick `Ctrl-X` and the local environment can be investigated[^1].
For a quick glance, the current local variables can be inspected in the top
right.

As good as this package is, by default it cannot be used with `pytest` as it
requires `stdin` to be passed, and `stdout` to be shown. By default `pytest`
captures `stdout`. There are two solutions:

1. run `pytest` with the `--nocapture` flag, but this will show all console
   output, or
2. install `pytest-pudb` which transparently opens up `pudb` at your embed
   point.

[^1]: similar to another handy snippet: `import IPython; IPython.embed()` which
  embeds an IPython session in the current running script.
