The ADM Health Check provides a generic overview of the ADM models in your system, including charts like the "Bubble Chart" and many more, with recommendations. It can be used as-is or be used as a basis for further, specific analyses. The ADM Model Reports give a detailed view of an individual ADM model, including the binning details of all the predictors which can help drive insights.

Technically, both are Quarto markdown documents using snippets of Python code from the PDS Tools libraries. We also have legacy R versions of both (using R markdown instead of Quarto).

Whatever option you choose, the ADM datamart needs to made available first. It consists of two tables: one for the models, one with the predictor details - that latter one is optional for the ADM overview. For instructions on how to export the data from Pega, see [How to export the ADM Datamart](How-to-export-and-use-the-ADM-Datamart).

There are a number of options to generate the Health Check reports:

|Option|When to use|Instructions|
|---|---|---|
|Github Codespace|Run Health Check in the cloud, directly from GitHub without the need to install any tools|Instructions [below](#github-codespace-option)|
|Standalone Python app|Run a python-based application locally, need to install Python, PDS tools and supporting libraries, but no coding skills required|See [instructions on the Health Check application](#using-the-stand-alone-health-check-application)|
|VSCode or other IDE|Run the notebooks from a developer environment. Allows for customizations but requires coding skills.|Instructions below [for the R versions](#running-the-r-reports-from-rstudio)|
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

1. Click on the green Code button and choose to add a codespace (you'll need to sign in to GitHub, you can create a free account). It will now automatically set up the code space, run a browser-based version of VSCode, load PDS Tools and start the Health Check application. This will take a few minutes.

1. Ensure that pop ups are allowed, as the Health Check application will open in a new browser tab

1. Before accessing the app, first upload your ADM Datamart files to your GitHub workspace. You can accomplish this by right-clicking within the Explorer section (located on the left-hand side of the interface) and selecting 'Upload'. After the files are uploaded, navigate to the 'Import File' section of the app. Ensure you select the 'Direct file path' option and input the path '/workspaces/pega-datascientist-tools'.

1. Follow the [instructions on the Health Check application](https://pegasystems.github.io/pega-datascientist-tools/Python/articles/HealthCheckSetUp.html#Using-the-App:-A-Step-by-Step-Guide)

# Using the stand-alone Health Check application

## What is it?

The stand-alone health check application makes it easy to create the ADM Health Check and the individual model reports. You will need to have python and install pdstools, but you do not need to run a (data science) environment, and there is no need to create a script, it is all configured from a UI.

## How to install

1. Install [Quarto](https://quarto.org) and [Pandoc](https://pandoc.org).

2. [Install Python and PDS tools](https://github.com/pegasystems/pega-datascientist-tools/wiki#installation) with app dependencies. If you already had an older version of PDS tools make sure to upgrade to the latest.

`pip install --upgrade 'pdstools[app]'`

3. Launch the Health Check application by running 

`pdstools run`

4. The app should open up in your system browser. On first run, you may get a promotional message from streamlit asking for your e-mail address - you can leave this empty if you want. If the app does not open up automatically, simply copy the Local URL from your terminal and paste it into your browser.

## How to use
 
1. In the app, navigate to the Health Check tab (in the left pane). This shows instructions.
 
1. Then click the "Data Import" tab in the main screen to load your data. If you haven't downloaded the ADM Datamart yet, this is the moment to do so. For instructions on how to export the data from Pega, see [How to export the ADM Datamart](How-to-export-and-use-the-ADM-Datamart). For a test drive, you can also skip uploading your own data, and select "CDH Sample" in the Data Import drop down.
   1. Use **Direct file path** with the folder path where the ADM files are located. Ex. _/User/Downloads/_. The tool should automatically find the relevant files in that directory. Note: there is no need to extract the zip files, we will also take care of that for you.
   2. Use **Direct file upload** to browse your local files.


1. The "Report Configuration" has a few advanced options but can generally be left empty

1. Then Generate and Download the ADM Health Check report. The download button will appear when the generation is finished. The downloaded report will show in your default browser download locations.


# Running the R reports from RStudio

1. Install [Quarto](https://quarto.org) and [Pandoc](https://pandoc.org).
2. Install R, R Studio and the pdstools package as per the ["Getting started" in the main page](https://github.com/pegasystems/pega-datascientist-tools/wiki#getting-started-with-the-r-library)
3. Either check out ("clone") the [Pega Data Scientist Tools repository](https://github.com/pegasystems/pega-datascientist-tools) from git, or (if you are not comfortable with git), just download the [Model Overview notebook](https://github.com/pegasystems/pega-datascientist-tools/blob/master/examples/datamart/healthcheck.Rmd).
4. From Pega, export the ADM datamart data (model data and optionally predictor data) as described in [How to export the ADM datamart data](How-to-export-and-use-the-ADM-Datamart).
5. Open the notebook "examples/datamart/healthcheck.Rmd" in R Studio and select "Knit with Parameters" from the Knit drop-down. 
6. Enter the full path to your ADM datamart downloads in the fields for *modelfile* and *predictordatafile*. If you do not have predictor data, make that field blank. 
7. Press the "Knit" button

When finished (it takes a few minutes), the report should open in a browser window. It will also be saved automatically in your working folder. This stand-alone HTML (or PDF) file can then easily be distributed.

| Knit option in R Studio | Knit dialog for the Health Check notebook |
| :---: | :---: |
| <img src="/pegasystems/pega-datascientist-tools/blob/master/images/R-studio-healthcheck-knit-with-params.png"> | <img src="/pegasystems/pega-datascientist-tools/blob/master/images/R-studio-healthcheck-knit-dialog.png"> |

Follow similar steps to manually create individual model reports. Select "Knit with parameters" and fill in the paths to the ADM datamart download and the ID of the model you want to report on. The ID can be found in the side panel from the Prediction Studio UI, or by loading the model data in R or Python and inspecting it there. The title and description are used in the generated file for informational purposes only.

| Knit option in R Studio | Knit dialog for the stand alone Adaptive Model Report notebook |
| :---: | :---: |
| <img src="/pegasystems/pega-datascientist-tools/blob/master/images/R-studio-modelreport-knit-with-params.png"> | <img src="/pegasystems/pega-datascientist-tools/blob/master/images/R-studio-modelreport-knit-dialog.png"> |


