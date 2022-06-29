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
# requirements.txt
geopandas==0.10.2
```

## Installation

With those files set, you can install the package via 

```bash
(pez_env) matt@matt:~/projects/pezzetti$ python -m pip install -e .
```

## Testing

Writing tests is a good way to make sure your code does what you expect.

### Additional Files and File Updates

While not strictly necessary, it's a good idea to make a `requirements_dev.txt` file, which pins the versions of packages used in the development of the package.

```txt
# requirements_dev.txt
flake8==3.9.2
tox==3.24.3
pytest==6.2.5
pytest_cov==2.12.1
mypy==0.910
```

#### Extending `setup.cfg` with testing and dev configs
And you'll also need to update `setup.cfg` with testing info. Append something like the following to your `setup.cfg` file.

```cfg
[options.extras_require]
testing =
    pytest>=6.0
    pytest_cov>=2.0
    mypy>=0.910
    flake8>=3.9
    tox>=3.24

[options.package_data]
pezzetti = py.typed

[flake8]
max-line-length = 160
```

The `py.typed` part indicates the package has been type-hinted, and it requires that you create an empty file named `py.typed` in any directory with an `__init__.py` file (ie any directory containing importable modules).

#### Extending `pyproject.toml` with testing and dev configs

Append configs (like those shown below) to the `pyproject.toml` to govern the behavior of tools like pytest or mypy.

```toml
...
[tool.pytest.ini_options]
addopts = "--cov=slapping"
testpaths = [
    "tests",
]

[tool.mypy]
mypy_path = "src"
check_untyped_defs = true
disallow_any_generics = true
ignore_missing_imports = true
no_implicit_optional = true
show_error_codes = true
strict_equality = true
warn_redundant_casts = true
warn_return_any = true
warn_unreachable = true
warn_unused_configs = true
no_implicit_reexport = true
```

### Workflow

#### install dev requirements

```bash
(pez_env) matt@matt:~/projects/pezzetti$ python -m pip install -r .\requirements_dev.txt
```

#### typechecking with `mypy`

To run `mypy` typechecking, run

```bash
(pez_env) matt@matt:~/projects/pezzetti$ python -m mypy pezzetti
```

#### linting with `flake8`

To run `flake8` linting, run

```bash
(pez_env) matt@matt:~/projects/pezzetti$ python -m flake8 pezzetti
```

If there are errors, they will be displayed. If there aren't linting errors, nothing will be displayed

#### Testing with `pytest`

To run `pytest` testing, run

```bash
(pez_env) matt@matt:~/projects/pezzetti$ python -m pytest
```

## Using pytest

You have to write tests for pytest to run.

Pytest looks through the test directory specified in the configs, then it looks for any module with a name that starts with `test_`, and for functions in those modules with names that start with `test_`.

### pytest decorators

* parameterizing function inputs.
    * You can use the `@pytest.mark.parameterize()` decorator to define parameters to pass in to a function.
* skipping functions
    * `@pytest.mark.skip(reason="whatever")`
    * `@pytest.mark.skipif(logic)`
* functions that should fail
    * `@pytest.mark.xfail`
* functions that should raise a specific error
    * use a context manager, like below

```python
def test_invalid_parameter(value: float):
    with pytest.raises(ValueError):
        operation_that_needs_ints(value)
```

### Sharing setup structures
Use Fixtures!

You can define a fixture within a script by using the `@pytest.fixture(scope="class|session|module|package")` decorator. If you want to share the fixture between many test modules, put the fixture in a module with the name `conftest.py`.
* Note: if you have a fixture that will require some teardown (ie anything from a context manager), use `yield` instead of `return`.

### Monkeypatch

If you need to temporarily modify some system functionality for a testing purpose. For example, if you needed to capture output that would otherwise print to the console via stdout, you could do something like 

```python
import pytest
import sys

@pytest.fixture
def capture_stdout(monkeypatch):
    buffer = {"stdout": "", "write_calls": 0}

    def fake_write(s):
        buffer["stdout"] = buffer["stdout"] + s
        buffer["write_calls"] = buffer["write_calls"] + 1

    monkeypatch.setattr(sys.stdout, "write", fake_write)
    return buffer
```

That temporarily replaces `sys.stdout`'s `.write()` method with the `fake_write()` method defined in `capture_stdout()`, and after the test that change is automatically undone.

You can use that monkeypatched fixture as below

```python
def test_print(capture_stdout):
    print("message")
    assert capture_stdout["stdout"] == "message\n"
```

## Testing the package with different versions of python or packages

You can test your package with many different versions of packages or python via the `tox` package's functionality.

### Config file: `tox.ini`

This is a `toml`-like file that will look like

```ini
[tox]
minversion = 3.8.0
envlist = py36, py37, py38, py39, flake8, mypy
isolated_build = true

[gh-actions]
python =
    3.6: py36, mypy, flake8
    3.7: py37
    3.8: py38
    3.9: py39

[testenv]
setenv =
    PYTHONPATH = {toxinidir}
deps =
    -r{toxinidir}/requirements_dev.txt
commands =
    pytest --basetemp={envtmpdir}

[testenv:flake8]
basepython = python3.6
deps = flake8
commands = flake8 src tests

[testenv:mypy]
basepython = python3.6
deps =
    -r{toxinidir}/requirements_dev.txt
commands = mypy src
```

the `py36`, `py37`, `py38`, and `py39` envs are built into `tox`, while the `flake8`, `mypy` envs are not and must be specified in the `[testenv:flake8]` and `[testenv:mypy]` blocks (respectively).

#### Running `tox`

After configuring things in the `tox.ini` file, run `tox` via 

```bash
(pez_env) matt@matt:~/projects/pezzetti$ tox
```

This may take a long time, so this may only be worth running before pushing changes to `main`.



































end