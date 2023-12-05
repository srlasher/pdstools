The ADM Health Check provides a generic overview of the ADM models in your system, including charts like the "Bubble Chart" and many more, with recommendations. It can be used as-is or be used as a basis for further, specific analyses. The ADM Model Reports give a detailed view of an individual ADM model, including the binning details of all the predictors which can help drive insights.

Technically, both are Quarto markdown documents using snippets of Python code from the PDS Tools libraries. We also have legacy R versions of both (in R markdown).

Whatever option you choose, the ADM datamart needs to be exported separately. For instructions on how to export the data, see [How to export the ADM Datamart](How-to-export-and-use-the-ADM-Datamart).

There are a number of options to generate the Health Check reports:

|Option|When to use|Instructions|
|---|---|---|
|Github Codespace|Run Health Check in the cloud, directly from GitHub without the need to install any tools|Instructions [below](#github-codespace-option)|
|Standalone Python app|Run a python-based application locally, need to install Python, PDS tools and supporting libraries, but no coding skills required|See [instructions on the Health Check application](https://pegasystems.github.io/pega-datascientist-tools/Python/articles/HealthCheckSetUp.html)|
|VSCode or other IDE|Run the notebooks from a developer environment. Allows for customizations but requires coding skills.|[Instructions for the R versions](#running-the-r-reports-from-rstudio)|
|Batch runs|Run the reports in batch - especially convenient when you need to create many individual model reports, or when doing this regularly. Supporting example batch scripts are available, requires some (light) scripting skills and an environment with Python, PDS tools and supporting libraries.|[Creating Reports in Batch](Batch‐creating-Adaptive-Model-Reports)|

# Github Codespace option

## What is it?

Run Health Check in the cloud, directly from GitHub without the need to install any tools. A [Github codespace](https://docs.github.com/en/codespaces) is a development environment that's hosted in the cloud. Each codespace you create is hosted by GitHub in a Docker container, running on a virtual machine.

## When would I use it?

* When you do not or cannot have Python or R installed locally on your computer.
* When you want to quickly run an ADM health check report with no need for local set up.

## What are the considerations?

* This analysis is not processed on your own device, you're running the code on GitHub.

* You will load your ADM snapshot data into the GitHub codespace. This data will be limited to the model and predictor data from ADM.
You can review the structure of the two datasets [here](https://docs-previous.pega.com/decision-management-reference-materials/database-tables-monitoring-models).
 
* You should review your corporate security policies and ensure that using GitHub cloud codespaces is permitted. Reference the [GitHub codespace security policies](https://docs.github.com/en/codespaces/reference/security-in-github-codespaces).

* Pega will not have visibility of any usage statistics/telemetry data/real data you load into this space.

## What are the steps to use it?

<img src="/pegasystems/pega-datascientist-tools/blob/master/images/codespace.png">

1. Navigate to the main [PDS Tools page on GitHub](https://github.com/pegasystems/pega-datascientist-tools).

1. Click on the green Code button and choose to add a codespace

1. Ensure that pop ups are allowed, as the Health Check application will open in a new browser tab

1. Follow the [instructions on the Health Check application](https://pegasystems.github.io/pega-datascientist-tools/Python/articles/HealthCheckSetUp.html#Using-the-App:-A-Step-by-Step-Guide)


# Running the R reports from RStudio

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



