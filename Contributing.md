We love feedback and contributions.

To file a bug, issue, ask a question or suggest an improvement, please use the Issues and Discussions facilities in GitHub. We will respond as soon as possible.

Use pull requests to submit additions or improvements that you have made and would like to share with others. While the code in this repository currently is mostly R and Python, we also welcome reusable BI reports from popular platforms, or code in upcoming languages like Julia.

We run continuous integration on this GitHub repository. This is configured through [GitHub Actions](https://docs.github.com/en/actions). Code coverage is provided through [codecov.io](https://app.codecov.io/gh/pegasystems/pega-datascientist-tools). Documentation is integrated into GitHub pages.

# Contributing to R

To contribute to the R package, please make sure to understand the basics of R packages. There are excellent tutorials on the web on [R package development](http://r-pkgs.had.co.nz/). A few quick tips:

* The R project for the package is in the "r" folder: `pega-datascientist-tools/r`, not the top-level folder.
* Use `library(devtools)`. This gives shortcuts to check the package with (Ctrl/Cmd + Shift + E), build and reload in a clean R session (Ctrl/Cmd + Shift + B), run all tests (Ctrl/Cmd + Shift + T), update doc and namespace file (Ctrl/Cmd + Shift + D), load package (Ctrl/Cmd + Shift + L).
* For unit tests, we use [testthat](https://testthat.r-lib.org), see `r/tests/testthat/tests_*.R` files
* Package documentation is done via [pkgdown](https://pkgdown.r-lib.org/). Run `pkgdown::build_site()` in R studio to build the `r/docs/` directory.
* Add new functions in `r/man/*.Rd`. 
* The GitHub pages docs live in the top-level docs folder. So after generating the package documentation, **move** that folder to `docs/R`. Best is to

```
git rm -rf docs/R
mv r/rocs docs/R
git add docs/R
```

* Code coverage is supported by the [covr](https://about.codecov.io/) package. To run code coverage locally, use

```
library(covr)
report()
```

* Examples are mostly in R vignettes which are part of the package and automatically documented, with HTML output available on GitHub. Some example notebooks and scripts are in the top-level `/examples` folder so outside of the R package. These will be checked locally when running the unit tests, but (currently) are not as part of the continuous integration process. Their output will also not be part of the generated documentation.
* The R package is installed directly from GitHub (see [Getting Started](/pegasystems/pega-datascientist-tools/wiki#getting-started-with-the-r-library) section in this Wiki), not currently via a package repository like CRAN.

# Contributing to Python

We prefer working in an IDE like [VSCode](https://code.visualstudio.com/) to work with the Python utilities and notebooks, but there are plenty of options.

In the text below where we use **pip**, depending on your local Python setup, you may need to replace this with **pip3**.

* Code is in Python 3 (3.6+). Continuous integration tests (configured as [GitHub Actions](https://docs.github.com/en/actions)) against a few different versions of Python and O/S.
* Tests are in [pytest](https://docs.pytest.org/). To run the tests locally, simply run `pytest` in the python folder. 
* Code coverage is part of the continuous integration. To run locally make sure to install the package first (`codecov` and `pytest-codecov`): 
```
pytest --cov=./python/pdstools --cov-report=xml
```
* The Python documentation uses [Sphinx](https://www.sphinx-doc.org/) to generate the docs, **nbsphinx** to convert the jupyter notebooks to markdown, and **Furo** as the Sphinx template. These dependencies can be installed with `pip install -r docs-requirements.txt`. [Pandoc](https://pandoc.org/) is a requirement too and needs to be installed separately.
* A **Makefile** is provided to create the documentation. Simply run
```
pushd python/docs
make html
popd
```
* The Python pdstools package is published to [PyPI](https://pypi.org/) and installable via pip as described in the [Getting Started](/pegasystems/pega-datascientist-tools/wiki#getting-started-with-the-python-tools). To uninstall the published version and install the local version instead:
```
pip uninstall pdstools
pip install .
```
* To create the distribution wheel and tar files, run `python -m build`. This creates the distribution files in the `dist` folder.

