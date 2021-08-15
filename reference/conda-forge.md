# conda-forge notes

## Why conda?

Conda exists to make it easy to produce binaries so that it's easy to install packages (including ones that use code across multiple languages that need to compile) in many different systems. 

## Adding a new package to conda-forge

1. fork the https://github.com/conda-forge/staged-recipes repo
2. Implement a recipe file (in `recipes/<your package's name>/meta.yaml`)
    One method
    1. Open `Anaconda Prompt`
    2. Activate an env that has the `conda-build` package
    3. Enter the command `conda skeleton pypi <your package's name>`


## Loose notes

`build.sh`
`$PREFIX` is the location `conda-build` goes to to look for packages when it's trying to resolve dependencies. (I think, per this [SciPy-2018 presentation](https://youtu.be/xiI1i525ljE?t=10519)).