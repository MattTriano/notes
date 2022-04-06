# conda-forge notes

## Why conda?

Conda exists to make it easy to produce binaries so that it's easy to install packages (including ones that use code across multiple languages that need to compile) in many different systems. 

Conda also has a dependency solver (unlike pip), which will help you avoid breaking your environment by checking that a new package's version dependencies don't conflict with the dependencies of already-installed packages. 

### How does conda solver work?

Requirements checks.

**build requirements**
Tools needed to build packages (and are not included in headers or linking files; c stuff?)

**host reqs**
External dependencies fo rthe package that need to be present at build time (python deps used in setup.py).

**run reqs**

## Adding a new package to conda-forge

1. fork the https://github.com/conda-forge/staged-recipes repo
2. Implement a recipe file (in `recipes/<your package's name>/meta.yaml`)
    One method
    1. Open `Anaconda Prompt`
    2. Activate an env that has the `conda-build` package
    3. Enter the command `conda skeleton pypi <your package's name>`


## Loose notes

`build.sh`
* `$PREFIX` is the location `conda-build` goes to to look for packages when it's trying to resolve dependencies. (I think, per this [SciPy-2018 presentation](https://youtu.be/xiI1i525ljE?t=10519)).
* `conda update` is used to update to the latest compatible version. `conda install` can be used to install any version.


### Current steps to make `conda skeleton pypi click` approximately work:
https://conda.io/projects/conda-build/en/latest/user-guide/tutorials/build-pkgs-skeleton.html

1. delete `.condarc` file and open a fresh terminal
2. create and activate a new env
    * `$ conda create --name smithy_env python=3.9`
    * `$ conda activate smithy_env`
3. (tutorial prerequisites) install conda-build
    * `$ conda install conda-build`
    * `$ conda update conda`
    * `$ conda update conda-build`
4. `$ cd ~/projects/conda_forging`
5. `$ conda skeleton pypi click --version 7.1.2`
6. `$ conda-build click`
7. `$ cd ~/miniconda3/envs/smithy_env/pkgs`
8. `$ conda install -c local click-7.1.2-py39_0.tar.bz2`

This gives me a `conda list` that looks like

`click                     7.1.2                    py39_0    <unknown>`

### Best current idea on process for updating a feedstock (still haven't gotten all the way to a working deployment)

1. Create a conda env with an updated version of `conda-build`
    * `$ conda create --name smithy_env`
    * `$ conda activate smithy_env`
    * `(smithy_env) $ conda install -c conda-forge python=3.9`
    * `(smithy_env) $ conda install -c conda-forge conda-build`
2. Fork the feedstock repo you want to update (if you haven't already), and clone it to your local system
3. (in the `<package_name>-feedstock/recipes/meta.yaml` file) Make your updates
    * For pypi installs (where the `source` `url` is on pypi), update 
        * the version number at the top,
        * the `sha256` checksum hash. To get the new hash:
            * go to the page for that package on pypi,
            * Go to the "Download files" page for the version you are updating to,
            * For the {package_name}-{version}.tar.gz file, click "View" in the **Hashes** column of the table,
        * Set the `number` in the `build` section to 0.
4. From the root dir of the feedstock, build the project
    * `(smithy_env) $ conda build .`

My most recent attempt (with the `missingno` package) failed as the `.tar.gz` source code file was not present on the [pypi page for that version](https://pypi.org/project/missingno/0.5.0/#modal-close). There was only the `.whl` file.


## Resources 

* [The docker hub image conda-forge uses in its feedstock CIs](https://hub.docker.com/r/condaforge/linux-anvil/)
* [LIGO conds-forge stype packaging docs](https://computing.docs.ligo.org/conda/packaging/)
* [Conda tutorial on building packages](https://docs.conda.io/projects/conda-build/en/latest/user-guide/tutorials/building-conda-packages.html)
* [`conda-build` explanation of the build process](https://docs.conda.io/projects/conda-build/en/latest/concepts/recipe.html)
* [conda-forge feedstocks (recipes to look at)](https://github.com/conda-forge/feedstocks/tree/main/feedstocks)
* [conda packages: concepts and basic usage](https://conda.io/projects/conda/en/latest/user-guide/concepts/packages.html)


# 2022_02_23 notes

It seems the most important page for understanding `conda-build` is the [defining metadata](https://docs.conda.io/projects/conda-build/en/latest/resources/define-metadata.html) documentation page.

A `conda-build` recipe requires:
* `meta.yaml` ---A file that contains all the metadata in the recipe. Only package/name and package/version are required.
* `build.sh`  ---The script that installs the files for the package on macOS and Linux. It is executed using the bash command.
* `bld.bat`   ---The build script that installs the files for the package on Windows. It is executed using cmd.
* `run_test.[py,pl,sh,bat]` ---An optional Python test file, a test script that runs automatically if it is part of the recipe.
* Optional patches that are applied to the source.
* Other resources that are not included in the source and cannot be generated by the build scripts. Examples are icon files, readme files and build notes.

* [highlighted sample recipes](https://docs.conda.io/projects/conda-build/en/latest/user-guide/recipes/sample-recipes.html)
    * python-only libraries:
        * [toolz](https://github.com/conda-archive/conda-recipes/tree/master/python/toolz)
            * build.sh
```bash
#!/bin/bash
mkdir -vp ${PREFIX}/bin;
touch requirements.txt;
${PYTHON} setup.py install || exit 1;
```
            * bld.bat
```bash
python setup.py install
if errorlevel 1 exit 1
```
            * meta.yaml
```YML
package:
    name: toolz
    version: 0.6.0

source:
  fn: toolz-0.6.0.tar.gz
  url: https://pypi.python.org/packages/source/t/toolz/toolz-0.6.0.tar.gz
  md5: 779bd5844a223c9f0be7e7ea03cc02b1

build:
    number: 0

requirements:
    build:
        - python
        - setuptools

    run:
        - python

test:
    imports:
        - toolz

    #commands:
    #    -

about:
    home: http://toolz.readthedocs.org/
    license: New BSD
```
        * [six](https://github.com/conda-archive/conda-recipes/tree/master/python/six)
            * build.sh
```bash
#!/bin/bash
$PYTHON setup.py install
```
            * bld.bat
```bash
"%PYTHON%" setup.py install
if errorlevel 1 exit 1
```
            * meta.yml
```bash
package:
  name: six
  version: "1.9.0"

source:
  fn: six-1.9.0.tar.gz
  url: https://pypi.python.org/packages/source/s/six/six-1.9.0.tar.gz
  md5: 476881ef4012262dfc8adc645ee786c4
#  patches:
   # List any patch files here
   # - fix.patch

# build:
  # noarch_python: True
  # preserve_egg_dir: True
  # entry_points:
    # Put any entry points (scripts to be generated automatically) here. The
    # syntax is module:function.  For example
    #
    # - six = six:main
    #
    # Would create an entry point called six that calls six.main()


  # If this is a new build for the same version, increment the build
  # number. If you do not include this key, it defaults to 0.
  # number: 1

requirements:
  build:
    - python
    - setuptools

  run:
    - python

test:
  # Python imports
  imports:
    - six

  # commands:
    # You can put test commands to be run here.  Use this to test that the
    # entry points work.


  # You can also put a file called run_test.py in the recipe that will be run
  # at test time.

  # requires:
    # Put any additional test requirements here.  For example
    # - nose

about:
  home: http://pypi.python.org/pypi/six/
  license: MIT License
  summary: 'Python 2 and 3 compatibility utilities
```