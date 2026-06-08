# Docs creation

In order to build the docs you need to

1. install sphinx and additional support packages
2. build the package reference files
3. run sphinx to create a local html version

The documentation is build using readthedocs automatically.

Install the docs dependencies of the package (as speciefied in toml):

```bash
# in main folder
pip install ".[docs]"
```

## Build docs using Sphinx command line tools

Command to be run from `path/to/docs`, i.e. from within the `docs` package folder:

Options:

- `--separate` to build separate pages for each (sub-)module

```bash
# pwd: docs
# apidoc
sphinx-apidoc --force --implicit-namespaces --module-first -o reference ../src/python_package
# build docs
sphinx-build -n -W --keep-going -b html ./ ./_build/
```

## Interactive notebooks

### colab 
- google product so requires users to have a google account
- does not set up the env automatically so in your notebook include commands for installing dependencies e.g.
    ```python
    # for colab
    !pip install python_package
    ```

#### Setup
- configure launch_buttons in html_theme_options
- for example:
    ```python
    html_theme_options = {
        ...
        "launch_buttons": {
            ...
            "colab_url": "https://colab.research.google.com",
        }
    ```

### binder
- doesn't require users to install things into env like colab does

#### Setup
- have repository_url and branch in html_theme_options of conf.py
- also configure launch_buttons in html_theme_options
    - [mybinder.org](https://jupyter.org/binder#binderhub) is a free service
    - or [deploy your own?](https://jupyter.org/binder#binderhub)
- for example:
    ```python
    html_theme_options = {
        ...
        "repository_url": "https://github.com/biosustain/python_package",
        "repository_branch": "main",
        "launch_buttons" : {
            ...
            "binderhub_url": "https://mybinder.org",
            "notebook_interface": "jupyterlab",
        }
    }
    ```


### thebe
- relies on binder
- turns notebooks to interactive code cells in the docs in the browser. 

#### Extra Requirements
- installation of [`sphinx-thebe`](https://sphinx-thebe.readthedocs.io/en/latest/) e.g. add sphinx-thebe to docs group of [pyproject.toml](../pyproject.toml)
- also add to `extensions` in sphinx conf.py

#### Setup
- configure thebe in html_theme_options in [conf.py](conf.py)
    ```python
    html_theme_options = {
        ...
       # for binder which is used by thebe
        "repository_url": "https://github.com/biosustain/python_package",
        "repository_branch": "main",
        "launch_buttons" : {
            ...
            "thebe": True
        }
    }
    ```
    - instead can configure thebe repository url and branch in `thebe_config`
- the `selector` in `thebe_config` tells thebe what html element to search for to tun into a runnable code cell 
- if wanting to make all code cells runnable set `selector` to `<div>`s with `class="highlight`
    ```python
    # thebe options
    thebe_config = {
    "selector": "div.highlight",
    "repository_url": "https://github.com/biosustain/python_package", # optional, will take from html_theme_options
    "repository_branch": "main", # optional    
    }
    ``` 
- by adding thebe to launch_buttons configuration, there will already be "▶️ Live Code" option from the launch button of the notebook-derived pages
- you can also add an [html button to trigger thebe](https://sphinx-thebe.readthedocs.io/en/latest/configure.html#adding-your-own-button-to-start-thebe)
    e.g. for sphinx-book-theme:
    this code: 
    ```markdown
    <button class="thebe-button" onclick="initThebeSBT()">Activate Notebook</button>
    ```
    produces:
    <button class="thebe-button" onclick="initThebeSBT()">Activate Notebook</button>


