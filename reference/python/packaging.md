# Writing Python Packages

There are a few package management tools in the python ecosystem, but the two I use most frequently are `conda` and `pip`.

`pip` is by far the most commonly used python package management tool and it pulls packages from [PyPI](https://pypi.org/) (the Python Packaging Index). The drawbacks of `pip` are
* it's limited just to packages writen purely in python,
* it's a chore to make environments that use different versions of python

The other package manager, `conda`, is the default package manager I use, which addresses both of the above issue. In the `conda` ecosystem, packages are hosted on `conda channels`. The default channel is maintained by Continuum Analytics and it only hosts a very limited subset of available python packages, but a community-led channel, [conda-forge](https://conda-forge.org/), makes a far, far larger subset of packages available to users.

## Packaging your python code for PyPI

PyPI makes it very easy (as well as free) to publish your python packages. You will need to register for an account (so that you can own a package name and keep others from overwriting your package code with malicious code), but that's free and easy.

After making an installable python package (see [this repo](https://github.com/MattTriano/mypkg) I made to demonstrate the process of creating and installing a python package), you can easily build your package using built-in python tools and publish it to PyPI using the `twine` package.

First, build your source distribution file, then publish it

```bash
(dev_env) user@host: ~/.../yourpythonpkg$ python setup.py sdist bdist_wheel
(dev_env) user@host: ~/.../yourpythonpkg$ twine upload dist/*
```

After entering your PyPI credentials, your package will upload and will immediately be installable by anyone via `pip install <your_packages_name>`.

# More Modern Packaging

*these notes are largely taken from this mCoding YouTube video named [Automated Testing in Python with pytest, tox, and GitHub Actions](https://youtu.be/DhUpxWjOhME?t=307)*

## Files needed:
1. `pyproject.toml`
2. `setup.py`
3. `setup.cfg`
4. `requirements.txt`


### 1. `pyproject.toml`

example:

```toml
[build-system]
requires = ["setuptools>=42.0", "wheel"]
build-backend = "setuptools.build_meta"
```

This example config tells python to use tha standard `setuptools` packaging method which is based on a `setup.py` file.

### 2. `setup.py`

```python
from setuptools import setup

if __name__ == "__main__":
    setup()
```

The video asserts it's increasingly seen as a security risk to have too much build or installation code in your `setup.py` file (although I'm skeptical about that as major packages like [pandas](https://github.com/pandas-dev/pandas/blob/main/setup.py), [pytorch](https://github.com/pytorch/pytorch/blob/master/setup.py), [scikit-learn](https://github.com/scikit-learn/scikit-learn/blob/main/setup.py), and many other well engineered packages have massive `setup.py` files), and advises using this barebones `setup.py` implementation, which facilitates installing the package in `editable` mode (although, per a comment pinned by mCoding, "It seems you no longer need setup.py to install in editable mode if you’re using pip >= 21.3").

### 3. `setup.cfg`

This is where you extract package metadata and other details to.

```cfg
[metadata]
name = pezzetti
description = a package of little tools.
author = Matt Triano
license = MIT
license_file = LICENSE
platforms = unix, linux, osx, cygwin, win32
classifiers =
    Programming Language :: Python :: 3
    Programming Language :: Python :: 3 :: Only
    Programming Language :: Python :: 3.6
    Programming Language :: Python :: 3.7
    Programming Language :: Python :: 3.8
    Programming Language :: Python :: 3.9
    Programming Language :: Python :: 3.10

[options]
packages =
    pezzetti
install_requires =
    geopandas>=0.6
python_requires = >=3.6
package_dir =
    =pezzetti
```

### 4. `requirements.txt`

This is where you should pin specific versions needed to install the package or execute packaged code.

```txt
geopandas==0.10.2
```

## Installation

With those files set, you can install the package via 

```bash
(pez_env) matt@matt:~/projects/pezzetti$ python -m pip install -e .
```