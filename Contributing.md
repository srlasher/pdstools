We love feedback and contributions.

To file a bug, issue, ask a question or suggest an improvement, please use the Issues and Discussions facilities in GitHub. We will respond as soon as possible.

Use pull requests to submit additions or improvements that you have made and would like to share with others. While the code in this repository currently is mostly R and Python, we also welcome reusable BI reports from popular platforms, or code in upcoming languages like Julia.

We run continuous integration on this GitHub repository. This is configured through [GitHub Actions](https://docs.github.com/en/actions). Code coverage is provided through [codecov.io](https://app.codecov.io/gh/pegasystems/pega-datascientist-tools). Documentation is integrated into GitHub pages.

# Contributing to R

To contribute to the R package, please make sure to understand the basics of R packages. There are excellent tutorials on the web on [R package development](http://r-pkgs.had.co.nz/). A few quick tips:

* The R project for the package is in the "r" folder: `pega-datascientist-tools/r`, not the top-level folder.
* Use `library(devtools)`. This gives shortcuts to check the package with (Ctrl/Cmd + Shift + E), build and reload in a clean R session (Ctrl/Cmd + Shift + B), run all tests (Ctrl/Cmd + Shift + T), update doc and namespace file (Ctrl/Cmd + Shift + D), load package (Ctrl/Cmd + Shift + L).
* For unit tests, we use [testthat](https://testthat.r-lib.org), see `r/tests/testthat/tests_*.R` files
* Package documentation is done via [pkgdown](https://pkgdown.r-lib.org/). 
    - first remove current docs (`git rm -rf r/docs`) 
    - then run `pkgdown::build_site()` in R studio to build the `r/docs/` directory
    - then add these to git (`git add r/docs`)
* Add new functions in `r/man/*.Rd`. 
* The GitHub pages docs live in the `r/docs` folder. So after generating the package documentation, make sure they live in that folder. On an accepted pull request, the docs are automatically picked up and moved to the `gh-pages` branch, where they will be deployed.

* Code coverage is supported by the [covr](https://about.codecov.io/) package. To run code coverage locally, use

```
library(covr)
report()
```

* Examples are mostly in R vignettes which are part of the package and automatically documented, with HTML output available on GitHub. Some example notebooks and scripts are in the top-level `/examples` folder so outside of the R package. These will be checked locally when running the unit tests, but (currently) are not as part of the continuous integration process. Their output will also not be part of the generated documentation.
* The R package is installed directly from GitHub (see [Getting Started](/pegasystems/pega-datascientist-tools/wiki#getting-started-with-the-r-library) section in this Wiki), not currently via a package repository like CRAN.

# Contributing to Python

We prefer working in an IDE like [VSCode](https://code.visualstudio.com/) to work with the Python utilities and notebooks, but there are plenty of options. 

We prefer working with pull requests, because of CI/CD and release changelog automation reasons. While there are many ways to work with GitHub pull requests, perhaps the easiest way is to fork the repository, make changes in your fork, and then create a pull request from the github.com page of your own fork directly. It is also possible to use the GitHub Actions CI/CD with your local fork, but you need to manually enable it under the `Actions` tab in your fork.

In the text below where we use **pip**, depending on your local Python setup, you may need to replace this with **pip3**, or sometimes even `python3.x -m pip`.

## Tests

* Code is in Python 3 (3.8+). Continuous integration tests (configured as [GitHub Actions](https://docs.github.com/en/actions)) against a few different versions of Python and O/S.
* Tests are in [pytest](https://docs.pytest.org/). To run the tests locally, simply run `pytest` in the python folder. 
* Code coverage is part of the continuous integration. To run locally make sure to install the package first (`codecov` and `pytest-codecov`): 
```
pytest --cov=./python/pdstools --cov-report=xml
```

## Documentation & articles
* The Python documentation uses [Sphinx](https://www.sphinx-doc.org/) to generate the docs, **nbsphinx** to convert the jupyter notebooks to markdown, and **Furo** as the Sphinx template. These dependencies can be installed with `pip install -r docs-requirements.txt`. [Pandoc](https://pandoc.org/) is a requirement too and needs to be installed separately.
* A **Makefile** is provided to create the documentation. The docs are automatically generated on any push commits in the master branch, but you can build them locally by running:
```
pushd python/docs
make html
popd
```
* While the generated docs are automatically deployed, and will automatically pick up on changes in the docstrings for the API reference, the articles in the docs are not. To add articles to the documentation: 
1. Create an article somewhere in the top-level `examples` folder. These articles should be jupyter notebook files, and **should be empty, not pre-run**
2. Edit the `cp` command in `python/docs/Makefile` where the articles are copied to a temporary to include your article
3. Update `python/docs/source/Articles.rst` and add your entry to the included notebooks. Since they will be moved to the `articles` folder, simply enter the **name of the notebook file, without extension**. 
- For example, if we added an article called `Example.ipynb` in the `examples/helloworld` folder, we would add `../../examples/helloworld/Example.ipynb` to the cp line in the Makefile (right before `source/articles`, of course), and add `articles/Example` to the `python/docs/source/Articles.rst` file. To test that it works, we recommend building the docs locally before creating a pull request.

## Releases

* The Python pdstools package is published to [PyPI](https://pypi.org/) and installable via pip as described in the [Getting Started](/pegasystems/pega-datascientist-tools/wiki#getting-started-with-the-python-tools). To uninstall the published version and install the local version instead, clone the repo, navigate to the top-level `pega-datascientist-tools` folder, and run:
```
pip uninstall pdstools
pip install .
```
* PyPi releases are automated with tagged commits, but to create the distribution wheel and tar files locally, run `python -m build`. This creates the distribution files in the `dist` folder.

