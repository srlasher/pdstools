Many of the plot functions from the Pega Data Scientist Tools library are bundled in a single R notebook that performs a standard analysis of the models, propositions and predictors from the ADM datamart.

This notebook delivers some quick insights and typically leads to some follow up questions with the business owners. The table of contents of an example analysis is shown below:

<img src="/pegasystems/pega-datascientist-tools/blob/master/images/adm_health_check_toc.png" width="60%" height="60%">

The notebook is in R and will require R to be installed, but is just a tool and does not require any specific R skills.

# Running it on your own data

1. Install R, R Studio and the pdstools package as per the ["Getting started" in the main page](https://github.com/pegasystems/pega-datascientist-tools/wiki#getting-started-with-the-r-library)
2. Either check out ("clone") the [Pega Data Scientist Tools repository](https://github.com/pegasystems/pega-datascientist-tools) from git, or (if you are not comfortable with git), just download the [Model Overview notebook](https://github.com/pegasystems/pega-datascientist-tools/blob/master/examples/datamart/healthcheck.Rmd).
3. From Pega, export the ADM datamart data (model data and optionally predictor data) as described in [How to export the ADM datamart data](How-to-export-and-use-the-ADM-Datamart).
4. Open the notebook "examples/datamart/healthcheck.Rmd" in R Studio and select "Knit with Parameters" from the Knit drop-down. 
5. Enter the full path to your ADM datamart downloads in the fields for *modelfile* and *predictordatafile*. If you do not have predictor data, make that field blank. 
6. Press the "Knit" button

When finished (it takes a few minutes), the report should open in a browser window. It will also be saved automatically in your working folder. This stand-alone HTML (or PDF) file can then easily be distributed.

| Knit option in R Studio | Knit dialog for the Health Check notebook |
| :---: | :---: |
| <img src="/pegasystems/pega-datascientist-tools/blob/master/images/R-studio-healthcheck-knit-with-params.png"> | <img src="/pegasystems/pega-datascientist-tools/blob/master/images/R-studio-healthcheck-knit-dialog.png"> |

