ISA memory footprint study
==========================

A small study on the memory footprint size used by ISA-API for experimental
DAGs.

The results of this study are to go with the paper in preparation "ISA-API 
A Python library for creating, handling and publishing experimental metadata" 
to submit to *GigaScience*.

This study uses a snapshot taken from MetaboLights of all ISA-Tab metadata on
10 July 2017. This raw data is found in `data/raw/`.

**Use Python 3.6.**

`make data` runs two scripts.

First, it will run `src/data/make_dataset.py`. This takes the raw ISA-Tab
studies and attempts to load the studies using the ISA-API. If it fails to
load, it is excluded from further processing. If is succeeds the study files
are copied over into the `data/interim/` directory.

Secondly, it will run `src/data/make_memory_footprint_dataset.py`. This does the
analysis of memory size on the ISA-Tabs that we know can be loaded by the
ISA-API. For each ISA-Tab study, we load the study tables (study sample and
assay tables)
using Pandas to get the DataFrame for each table and calculate the memory size.
We then load the same study with ISA-API into ISA objects. We then calculate the
memory size of the process sequences that map to each of the study's table
files. The results are written out to `data/processed/`. You can find a data
dictionary spreadsheet table about the output in
`references/data_dictionary.csv`

Finally, we transform it into another table to be able to plot by category,
using `src/data/transform_to_categorized.py`.

For more detail on the data processing, please check the source code.

`make figures` generates some plots based on the output written to
`data/processed/`, writing out a series of PDF and PNG format figures into
`reports/figures/`. It uses the `src/visualization/make_figures.py` script.

Note: If the contents of `data/interim/` and `data/processed/` have not yet been generated, by running `make figures` it will implicitly run `make data` first. It may take some time to run the first time since it loads every MTBLS study twice (maybe takes about 60 minutes).

In the `notebooks/` directory you can find exploratory analyses and work
towards the code used for generating the figures. Relies on the data having been processed (using `make data`).

Project Organization
------------

    ├── LICENSE
    ├── Makefile           <- Makefile with commands like `make data` or `make figures`
    ├── README.md          <- The top-level README for using this project.
    ├── data
    │   ├── interim        <- Intermediate data that has been transformed.
    │   ├── processed      <- The final, canonical data sets for modelling.
    │   └── raw            <- The original, immutable data dump from MTBLS.
    │
    ├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
    │                         the creator's initials, and a short `-` delimited description, e.g.
    │                         `1-djcomlab-initial-data-exploration`.
    │
    ├── references         <- Data dictionaries, manuals, and all other explanatory materials.
    │
    ├── reports
    │   └── figures        <- Generated graphics and figures to be used in reporting
    │
    ├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
    │                         generated with `pip freeze > requirements.txt`
    │
    ├── setup.py           <- makes project pip installable (pip install -e .) so src can be imported
    ├── src                <- Source code for use in this project.
    │   ├── __init__.py    <- Makes src a Python module
    │   │
    │   ├── data           <- Scripts to generate data
    │   │   └── make_dataset.py
    │   │
    │   └── visualization  <- Scripts to create visualizations
    │       └── visualize.py
    │
    └── tox.ini            <- tox file with settings for running tox; see tox.testrun.org


--------

<p><sup>Project based on the <a target="_blank" href="https://drivendata.github.io/cookiecutter-data-science/">cookiecutter data science project template</a>.</sup></p>
