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