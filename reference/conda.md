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
            * For the <package_name>-<version>.tar.gz file, click "View" in the **Hashes** column of the table,
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