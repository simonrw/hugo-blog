---
title: "Modern Python development"
date: 2021-05-02T12:00:00+01:00
tags:
- python
summary: My recommendations for Python projects
---

Python dependency management is difficult.

...not because it's complicated, but rather because the ecosystem has been
around for so long, and evolved over time. This means that best
practices have come and gone. With that a great deal of confusion has
developed, from reading out of date blog posts or stack overflow advice.

I am here to hopefully simplify the process into the following steps:

1. [Install Python](#install-python): [`asdf`][asdf]
2. [Managing global packages](#managing-global-packages): [`pipx`][pipx]
2. [Install a Python dependency manager](#install-a-python-dependency-manager): [`poetry`][poetry]
3. [Best practices](#best-practices)
4. [My workflow](#my-workflow)

Throughout this post, I will explain my reasoning behind my suggestions,
so that hopefully you will understand **why** I have made my choices.

![The python ecosystem](/img/virtualenvs.svg)

*The Python ecosystem. Items in green represent suggested tools. Items
in red represent parts of the system that should not be used.*

## Install Python

**Suggestion**: Use [`pyenv`][pyenv] or [`asdf`][asdf]

[`pyenv`][pyenv] allows multiple versions of Python to be installed at
the same time, and selected per directory. This means that if a project
depends on Python 3.6 and another depends on Python 3.8, the two can be
designated per project.

[`asdf`][asdf] can be thought of as `pyenv` but for many languages, and
so is interchangeable.

The main reason I suggest using one of these two package managers is
then your install isn't tied to the system Python.

On macOS it's tempting to use [`homebrew`][homebrew] to install your
Python version. This is not a good idea.  [`homebrew`][homebrew]
regularly updates the `python` formula to include the latest version. If
for example, a CLI package is installed - such as the AWS CLI, it's tied
to a single global Python minor version. When [`homebrew`][homebrew]
updates the Python formula, the AWS CLI will stop working.

## Managing global packages

**Suggestion**: Use [`pipx`][pipx]

[`pipx`][pipx] is a tool which installs a single binary - for example
the AWS CLI - into its own virtual environment. This isolates **its**
packages from everything else. When combined with [`asdf`][asdf], this
means that Python version upgrades are up to you, and therefore will not
break your packages at unpredictable times.

This isolation is **especially** true for Python dependency managers.
They should be installed in a unique environment so that they don't pick
up packages from outside by accident.

## Install a Python dependency manager

**Suggestion**: Use [`poetry`][poetry] installed via [`pipx`][pipx]

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
  from one project are not installed into other projects; and
* package dependencies must be strictly resolved, so that projects can
  include the correct versions.

This is implemented through "virtual environments"[^2], which configure
a python interpreter to look in a specific location for packages. This
means that a virtual environment for project A can include different
packages than the virtual environment for project B.

### Project isolation

**Suggestion**: Use [`poetry`][poetry]

*If you are using pipenv, see the [pipenv](#using-pipenv)
section.*

Project dependencies should be considered in a separate way to global
packages. The same requirement for isolation is shared however.

Project dependency management introduces an additional layer of
complexity: version locking. For application-type projects (CLIs, web
applications, anything that is not going to be imported as a package by
others), package versions should be locked to ensure reproducible
behaviour. Library type projects, which are designed to be imported by
others - for example `requests` - should *not* lock their package
versions. The dependency specification of library type projects should
be as loose as possible, to reduce complications for projects that
import the library.

Note: this does not mean that version constraints cannot be used, but
rather that they should be as loose as possible.  Typically this means
using greater-than constraints rather than less-than.

Two major project dependency managers exist for Python:
[`poetry`][poetry] and [`pipenv`][pipenv].

The behaviour of these two project dependency managers is approximately
the same.  They loose specification for project dependencies, which is
then locked into a separate file, with exact version constraints.
Projects can be set up to install dependencies from this lockfile only
to ensure the same dependency versions each time the project is used.

Both projects create a virtual environment behind the scenes. This is
effectively invisible to the user. In order to run commands from within
that environment, the command must be prefixed by `<pipenv or poetry>
run <cmd>`. Both commands include a way to configure the current shell
to include those packages by default, for convenience: `<pipenv or
poetry> shell`.

The reasons to prefer [`poetry`][poetry] over [`pipenv`][pipenv] may
include an element of subjectivity, but here I will remain objective.

The first advantage of [`poetry`][poetry] is that the manifest format
used to define packages: a `pyproject.toml` file. This converts an
imperative script that must be executed (`setup.py`) in order to
determine a projects' dependencies, to a static manifest that can be
cached, or parsed without installing the package itself. This manifest
file is [already supported by pip > 19.0, from
2019](https://pip.pypa.io/en/stable/news/#v19-0) so legacy projects can
still use a dependency that has converted to `pyproject.toml`.  **This
is not the case for [`pipenv`][pipenv]. Its package format is custom and
exclusive to [`pipenv`][pipenv].**

[`poetry`][poetry] includes a custom dependency resolver, which is
faster than [`pipenv`][pipenv]'s. This makes a difference when getting
up and running with a new project, but also when adding new packages.

When working on a project, typically you want the local package to be
installed in "editable" mode - i.e. a symlink, so that package changes
are picked up when running tests. An alternative is to use [`tox`][tox],
however this is often slower than running tests against the local
package installed in editable mode, as a source package must be
generated and installed on each test run.

[`poetry`][poetry] does this by default. You do not need to specify the
local package in the manifest file, and [`poetry`][poetry] knows what to
do.

[`pipenv`][pipenv] supports installing the local package in development
mode, but in order to install the local package when "deploying"[^1],
the local package must be included in the `[packages]` section. In my
experience, this does not work. I believe packages in the `[packages]`
section overwrites packages in the `[dev-packages]` section, meaning
that the local package does not get installed in editable mode, and
changes are not reflected when running tests.

[`pipenv`][pipenv] does not have any support for publishing packages.
This means that if you use [`pipenv`][pipenv] to manage a library
project, you *must* use `setup.py` to define your library dependencies,
rather than `pyproject.toml`. [`poetry`][poetry] [supports building and
publishing packages as part of it's default feature
set](https://python-poetry.org/docs/libraries/)

## Best practices

* Set the `PIP_REQUIRE_VIRTUALENV` environment variable to `true`. This
  will prevent `pip` from installing into the global package list,
  without a virtual environment.
* **Never** install packages using `pip` into the global environment of
  a Python installation. [`pipx`][pipx] enforces the use of virtual
  environments for CLIs, and [`poetry`][poetry] enforces the use of
  virtual environments for projects.
* **Never** use sudo to install packages with `pip`. This practice
  usually indicates that you are doing something wrong.
* If using [`pipenv`][pipenv], read the output of the command when it
  runs, to make sure you are using an expected version of Python for the
  virtual environment. See the [pipenv section](#using-pipenv) for more
  details.
* Do not install packages with your system package manager (e.g.
  `apt-get`). These packages are usually installed only for the system
  Python, and you should not use the system Python.
* If you find yourself using `pip install --user` then something is
  wrong. If you are installing a CLI, then use [`pipx`][pipx]. If you
  are installing to a project, use the project's dependency manager.

## My workflow

My workflow when setting up a new laptop is as follows:

### Install `asdf`

1. `git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch <branch e.g. v0.8.0>`
2. add `source ${HOME}/.asdf/asdf.sh` to your shell
3. add python as a language: `asdf plugin add python`
4. install some python versions: `asdf install python 3.8.9 3.9.1`

### Install `pipx`

1. `brew install pipx`
2. Set up pipx to use a fixed version of Python: set the environment
   variable e.g. `PIPX_DEFAULT_PYTHON=$(asdf which python3.9)`

### Use `pipx` to install `poetry`

1. `pipx install poetry`

### Set up a Python project

1. `poetry init`
2. Add dependencies: `poetry add ...`

## Using pipenv

### Virtual environment version

If using [`pipenv`][pipenv], you must be careful when [`pipenv`][pipenv]
sets up the virtual environment for you.  It is fairly greedy on which
python it picks to use, so I usually create the virtual environment
manually before running any other [`pipenv`][pipenv] commands. 

In the example below, I set the local python version to 3.6.13 via
[`asdf`][asdf]. I then create a [`pipenv`][pipenv] project, but it
chooses to use python 3.9.5 to create the virtual environment rather
than 3.6.13 as I specify.

```
$ asdf local python 3.6.13
$ python --version
Python 3.6.13

$ which python
/Users/<me>/.asdf/shims/python

$ asdf which python
/Users/<me>/.asdf/installs/python/3.6.13/bin/python

# This step uses python 3.9 to create the virtual environment
$ pipenv install
Creating a virtualenv for this project...                  HERE
Pipfile: /private/tmp/pipenvtesting/Pipfile                ▼▼▼▼▼
Using /Users/<me>/.asdf/installs/python/3.9.5/bin/python3 (3.9.5) to create virtualenv...
⠼ Creating virtual environment...created virtual environment CPython3.9.5.final.0-64 in 245ms
  creator CPython3Posix(dest=/Users/<me>/.local/share/virtualenvs/pipenvtesting-TqUem2-j, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/Users/<me>/Library/Application Support/virtualenv)
    added seed packages: pip==21.0.1, setuptools==56.0.0, wheel==0.36.2
  activators BashActivator,CShellActivator,FishActivator,PowerShellActivator,PythonActivator,XonshActivator
✔ Successfully created virtual environment!
Virtualenv location: /Users/<me>/.local/share/virtualenvs/pipenvtesting-TqUem2-j
Installing dependencies from Pipfile.lock (3cbc06)...
  🐍   ▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉ 0/0 — 00:00:00
To activate this project's virtualenv, run pipenv shell.
Alternatively, run a command inside the virtualenv with pipenv run.
```

This is done by setting the python version for the project with asdf:
`asdf method local python 3.9.5`, and creating the [`pipenv`][pipenv]
virtual environment: `pipenv --python $(asdf which python)`. In
addition, it should be common practice to specify the python version in
the `Pipfile`:

```
[requires]
python_version = "3.8"
```

If the version of python is not found with this configuration,
[`pipenv`][pipenv] will prompt you to install it, via either
[`asdf`][asdf] or `pyenv`.

Another common problem with [`pipenv`][pipenv] is locking packages with
Python >= 3.8, and trying to install the project with Python < 3.8. This
causes a confusing error:

```
ModuleNotFoundError: No module named 'importlib_metadata'
```

Python >= 3.8 includes `importlib-metadata` as part of the standard
library. Therefore, locking the packages with a later version of Python
does not include this package in the list of dependencies.

[^1]: For example building a docker container, or lambda.
[^2]: Until the [`__pypackages__` concept](https://www.python.org/dev/peps/pep-0582/) is more widely supported.

[asdf]: https://github.com/asdf-vm/asdf
[pyenv]: https://github.com/pyenv/pyenv
[poetry]: https://python-poetry.org/
[pipx]: https://github.com/pipxproject/pipx
[homebrew]: https://brew.sh
[pipenv]: https://pipenv.pypa.io/en/latest/
[tox]: https://tox.readthedocs.io/en/latest/

