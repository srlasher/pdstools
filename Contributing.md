We love feedback and contributions.

To file a bug, issue, ask a question or suggest an improvement, please use the Issues and Discussions facilities in GitHub. We will respond as soon as possible.

Use pull requests to submit additions or improvements that you have made and would like to share with others. While the code in this repository currently is mostly R and Python, we also welcome reusable BI reports from popular platforms, or code in upcoming languages like Julia.

We run continuous integration on this GitHub repository. This is configured through GitHub Actions. Code coverage is provided through [codecov.io](https://app.codecov.io/gh/pegasystems/pega-datascientist-tools). Documentation is integrated into GitHub pages.

# Contributing to R

To contribute to the R package, please make sure to understand the basics of R packages. There are excellent tutorials on the web on [package development](http://r-pkgs.had.co.nz/). A few quick tips:

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


# Contributing to Python

We prefer working in an IDE like VSCode to work with the Python utilities and notebooks, but there are plenty of options.

* Code is in Python 3 (3.6+). Continuous integration tests against a few different versions of Python and O/S.
* Tests are in **pytest**. To run the tests locally, simply run pytest in the python folder. 
* Code coverage is provided via the pytest-cov plugin, which is executed as part of the continuous integration. To run locally, 
```
pytest --cov=./python/pdstools --cov-report=xml
```
* The Python documentation uses **Sphinx** to generate the docs, **nbsphinx** to convert the jupyter notebooks to markdown, and **Furo** as the Sphinx template. These dependencies can be installed with `pip install -r docs-requirements.txt`. **Pandoc** is a requirement too and needs to be installed separately.
* A **Makefile** is provided to create the documentation. Simply run
```
cd python/docs
make html
```






