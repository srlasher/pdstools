Many of the plot functions from the CDH Tools library are bundled in a single R notebook that performs a standard analysis of the models, propositions and predictors from the ADM datamart.

This notebook delivers some quick insights and typically leads to some follow up questions with the business owners. The table of contents of an example analysis is shown below:

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/adm_health_check_toc.png" width="60%" height="60%">

Although the notebook is in R and will require R to be installed, it is just a tool and does not require any R skills.

# Sanity check

Before you run it on your own data, check that all is working by running the example:

1. Install R and R Studio as per the ["Getting started" in the main page](https://github.com/pegasystems/cdh-datascientist-tools/wiki#getting-started-with-the-r-library)
2. Either check out ("clone") the [CDH Tools repository](https://github.com/pegasystems/cdh-datascientist-tools) from git, or (if you are not comfortable with git), just download the [Model Overview notebook](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/examples/datamart/healthcheck.Rmd).
3. Open the notebook "examples/datamart/healthcheck.Rmd" in R Studio and "Knit to HTML" (or PDF if you have the required libs installed). When finished (it takes a few minutes), a sample report should open in a browser window.

# Running it on your own data

1. From Pega, export the ADM datamart data (model data and optionally predictor data) as described in [How to export the ADM datamart data](How-to-export-and-use-the-ADM-Datamart).
2. In R Studio, "Knit with Parameters" and specify the location of your downloads.

This can take a few minutes and will then give you a stand-alone HTML (or PDF) file that you can distribute.
