# Jupyterlab

### How to make hidden files visible

1. Generate a jupyter config file
    * `$ jupyter notebook --generate-config`
2. Open up the generated file (named `jupyter_notebook_config.py`, typically in `~/.jupyter`)
    * `$ cd ~/.jupyter && nano jupyter_notebook_config.py`
3. Go down to the **ContentsManager** section to the commented line `#c.ContentsManager.allow_hidden = False`, uncomment the line, change `False` to `True`, and save the file
4. Restart your jupyter server.

Hidden files should now be visible! Don't worry about the fact that the generate command indicates `notebook` instead of `lab`, it works for `jupyterlab` just fine.