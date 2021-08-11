# conda-forge notes

## Adding a new package to conda-forge

1. fork the https://github.com/conda-forge/staged-recipes repo
2. Implement a recipe file (in `recipes/<your package's name>/meta.yaml`)
    One method
    1. Open `Anaconda Prompt`
    2. Activate an env that has the `conda-build` package
    3. Enter the command `conda skeleton pypi <your package's name>`
