---
title: "Modern Python development"
date: 2021-05-02T12:00:00+01:00
tags:
- python
summary: How I set up Python projects
---

Python dependency management is difficult.

...not because it's complicated, but rather because the ecosystem has been
around for so long, and evolved over time. This means that best
practices have come and gone. With that a great deal of confusion has
developed, from reading out of date blog posts or stack overflow advice.

I am here to hopefully simplify the process into the following steps:

1. Install Python - [`asdf`][asdf]
2. Install a Python dependency manager - [`poetry`][poetry] +
   [`pipx`][pipx]
3. Best practices

Throughout this post, I will explain my reasoning behind my suggestions,
so that hopefully you will understand **why** I have made my choices.

## Install Python

**Suggestion**: Use [`pyenv`][pyenv] or [`asdf`][asdf]

[`pyenv`][pyenv] allows multiple versions of Python to be installed at
the same time, and selected per directory. This means that if a project
depends on Python 3.6 and another depends on Python 3.8, the two can be
designated per project.

[`asdf`][asdf] can be thought of as `pyenv` but for many languages, and
so is interchangable.

The main reason I suggest using one of these two package managers is
then your install isn't tied to the system Python.

On macOS it's tempting to use [`homebrew`][homebrew] to install your
Python version. This is not a good idea. [`homebrew`][homebrew]
regularly updates the `python` formula to include the latest version. If
for example, a CLI package is installed - such as the AWS CLI, it's tied
to a single global Python version. When [`homebrew`][homebrew] updates
the Python formula, the AWS CLI will stop working.

## Install a Python dependency manager

Python has a long and confusing history when it comes to packaging.
Unlike nodejs, go or rust, Python cannot import multiple versions of the
same package at the same time.

Python includes a variable `sys.modules` which is a dictionary of module
name to module. Even if a package could be installed using different
versions, Python would only refer to it once.

Restricting environments to use only a single package version means that
all versioning constraints must be met for a project's dependencies to
be installed.

This all therefore means that we must:

* isolate projects from each other, so that invalid versions of packages
  from one project are not installed into others; and
* package depdencies must be strictly resolved, so that projects can
  include the correct versions.

Historically this has been implemented through "virtual environments",
which configure a python interpreter to look in a specific location for
packages. This means that a virtual environment for project A can
include different packages than the virtual environment for project B.

This isolation is where my first suggestion in this section comes in:
[`pipx`][pipx].

[`pipx`][pipx] is a tool which installs a single binary - for example
the AWS CLI - into its own virtual environment. This isolates **its**
packages from everything else. When combined with [`asdf`][asdf], this
means that Python version upgrades are up to you, and therefore will not
break your packages at unpredictable times.

This isolation is **especially** true for Python dependency managers.
They should be installed in a unique environment so that they don't pick
up packages from outside.

## Best practices

My workflow when setting up a new laptop is as follows:

### Install `asdf`

1. `git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.8.0`
2. add `source ${HOME}/.asdf/asdf.sh` to your shell
3. add python as a language: `asdf plugin add python`
4. install some python versions: `asdf install python 3.8.9 3.9.1`

### Install `pipx`

1. `brew install pipx`
2. Set up pipx to use a fixed version of Python: set the environment
   variable `PIPX_DEFAULT_PYTHON=$(asdf which python3.9)`

### Use `asdf` to install `poetry`

1. `pipx 

[asdf]: https://github.com/asdf-vm/asdf
[pyenv]: https://github.com/pyenv/pyenv
[poetry]: https://python-poetry.org/
[pipx]: https://github.com/pipxproject/pipx
[homebrew]: https://brew.sh
