# Welcome to the Customer Decision Hub Data Scientist Tools Wiki!

These open source tools provides Data Scientists tools to analyze the performance of applications based on Pega AI, machine learning and decisioning applications, such as implementations of Pega Customer Decision Hub or Pega Process AI.

Tooling is both in R and Python although currently not everything is available in both languages. The current functionality includes

* Functions in [R](https://pegasystems.github.io/cdh-datascientist-tools/R/reference/index.html) and [Python](https://pegasystems.github.io/cdh-datascientist-tools/Python/autoapi/index.html) to read in data from the ADM Datamart, from Pega datasets and to easily create plots. 
* Collection of sample plots/graphs for analysis of CDH. See our [CDH Graph Gallery](CDH-Graph-Gallery).
* An [all-in-one "Health Check" notebook](ADM-Datamart-Health-Check) that can be applied to the ADM Datamart to get insights into the models and predictors.
* Create stand-alone and off-line browsable Adaptive model reports in HTML or PDF. [Step-by-step tutorial to create offline model reports](Create-stand-alone-Adaptive-Model-Reports).
* Example analysis of the ADM Datamart: [R](https://pegasystems.github.io/cdh-datascientist-tools/R/articles/adm-datamart.html) [Python](https://pegasystems.github.io/cdh-datascientist-tools/Python/articles/Example_ADM_Analysis.html). For instructions on how to export the data, see [How to export and use the ADM Datamart](How-to-export-and-use-the-ADM-Datamart).
* Example analysis of the Historical Dataset: [R](https://pegasystems.github.io/cdh-datascientist-tools/R/articles/historical-dataset.html) [Python](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/examples/ih/Example_IH_Analysis.ipynb).
* Example of a Value Finder analysis: [R](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/examples/valuefinder/vf_analysis.Rmd) [Python](https://pegasystems.github.io/cdh-datascientist-tools/Python/articles/vf_analysis.html)
* Freeze Adaptive models into PMML Scorecards including reason codes for decision explanations. [Step-by-step tutorial to create PMML Scorecards from ADM](Create-PMML-from-ADM-Models)
* Restore Adaptive models from model snapshots (when "full auditability" is switched on). [Example notebook](https://pegasystems.github.io/cdh-datascientist-tools/R/articles/snapshots-to-scorecards.html)
* More examples in the [R vignettes](https://pegasystems.github.io/cdh-datascientist-tools/R/articles/index.html) and the [Python documentation](https://pegasystems.github.io/cdh-datascientist-tools/Python/examples.html).

# Getting Started with the R library

## Installation

The `cdhtools` package can be installed straight from GitHub. First you will need R and R Studio.

1. Install R
    If you do not have R installed, go to https://www.r-project.org/ and install the latest version of the software. Find the correct installer for your platform (e.g. R-4.0.3.pkg) and follow the steps of the installer.

2. Install R Studio
    Install R Studio from [rstudio.com](https://rstudio.com/products/rstudio/). Follow the installation steps, then launch R Studio. On first launch, it should find R automatically, otherwise you will need to configure. We don't recommend installing with Homebrew (Mac) because this seems to default all packages to install from source which can cause trouble.

3. To install `cdhtools` from GitHub use the `devtools` package. If you don't have that installed yet, do that first:

```r
install.packages("devtools")
```

4. Then load the `devtools` library and install the `cdhtools` package. Note the `build_vignettes` flag. When prompted for updates, we recommend installing only binary CRAN packages. Note that we have seen failures when installing over VPN, so you may have to (temporarily) go off VPN to install the package from GitHub.

```r
library(devtools)
install_github("pegasystems/cdh-datascientist-tools/r", build_vignettes=TRUE)
```

## Using the R package

If all is well, this will install an R package called `cdhtools` that you can then use just like any other R package. The package contains help and vignettes to help you get going, both also accessible from the Github page directly. You can quickly check this by running the following R commands. This should show you an overview of the vignettes (package help), opens one of them and shows generic package help. 

```r
library(cdhtools)

?cdhtools
browseVignettes("cdhtools")
vignette(topic="adm-datamart")
```

The package ships with some data. Create your first plots with just a few lines:

```r
library(cdhtools)
library(ggplot2)
library(data.table)

data("adm_datamart")

plotPerformanceSuccessRateBubbleChart(adm_datamart)
```
<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/gettingstartedRplot1.png" width="50%">

You can add standard `ggplot` decoration to the returned plots, and most of the plot functions also have parameters to select the fields to aggregate or facet by, see help.

To get an overview of the 20 most important predictors in all the models:

```r
plotPredictorImportance(adm_datamart, limit = 20)
```

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/gettingstartedRplot2.png" width="50%">


To run the R examples you do not need to clone [the repository](https://github.com/pegasystems/cdh-datascientist-tools), but for some of the example files you do.


## Package Overview

The R package currently contains 

- Utilities to read data into `data.table` structures. The `ADMDatamart` function is the main work horse here, this reads downloaded Pega dataset exports as well as many other formats.
- Standard functions to easily create plots from Pega data, primarily from the ADM datamart. These plot functions can be further customized.
- Standard notebooks to generate off-line viewable reports that can be saved as stand-alone HTML or PDF files. There is one that strings together many of the plot functions to generate one big "datamart scan" including a lot of useful analyses. Another one creates a stand-alone model report similar to the view in Pega Prediction Studio, with all the predictor binning, the model classifier etc. These notebooks can easily be applied on any ADM datamart export.
- An (experimental) utility to take an ADM model and transform it into PMML. This PMML is basically a "frozen" version of the ADM model with each model instance represented as as Score Card including reason codes that can be used to explain the decision.

You can get the list of vignettes with `browseVignettes("cdhtools")` (as a web page) or `vignette(package="cdhtools")`. A vignette provides the original source as well as a readable HTML or PDF page and a file with the R code. Read a specific one with `vignette(x)` and see its code with `edit(vignette(x))`.

The other option is to download the source (clone from [the GitHub repository](https://github.com/pegasystems/cdh-datascientist-tools)) and use the functions and demo scripts directly. The R code, tests, vignettes etc are in the `r` subdirectory.

A reference to the available functions is also published on GitHub: [Function Reference](https://pegasystems.github.io/cdh-datascientist-tools/R/reference/index.html).

## Contributing

See [Contributing](Contributing#contributing-to-r)


# Getting Started with the Python tools

CDH Tools is intended to be used with **Python 3.6 and up**.

## Installation

```python
pip3 install git+https://github.com/pegasystems/cdh-datascientist-tools.git
```
Note: If you run this within a Jupyter notebook cell, be sure to add an exclamation mark in front of the command.

Alternatively, you could clone the repository through GitHub.

### Dependencies
- Pandas for the data structure
- Plotly >= 5.5.0 for the interactive visualisations
- Seaborn for the static visualisations
- Requests for importing data through a direct link
- Jupyterlab for jupyter notebook support
- Ipywidgets for interactive notebook widgets

### Optional dependencies
- Pyarrow for faster data loading
- Sklearn for AUC calculation in cdh_utils

### Compatibility
The current version of the Python files is tested on Python versions 3.6, 3.8 and 3.10. We recommend using plain notebooks or notebooks within VSCode's notebook editor.

## Using the Python tools

After installation, you can import the **ADMDatamart** class from the cdhtools package as such:
```python
from cdhtools import ADMDatamart
```
The ADMDatamart class is the main class which orchestrates reading, preprocessing and visualizing of the data. For an overview of its methods, please refer to the API reference. Normally, if the dataset is exported as per [the instructions page](https://github.com/pegasystems/cdh-datascientist-tools/wiki/How-to-export-and-use-the-ADM-Datamart), the only path you would need to give the ADMDatamart class is the directory of that data, and it should find the filenames itself. If, for some reason, those are changed, you can of course supply those names with the 'model_filename' and 'predictor_filename' arguments. This syntax also works for subdirectories: if models.csv is in a subfolder within the datamart folder named 'modeldatafile', the argument for model_filename would be 'modeldatafile/models.csv'.


```python
simple_data = ADMDatamart("~/Downloads/CDHSample")
custom_file_names = ADMDatamart("~/Downloads/CDHSample",
    model_filename = 'models.csv',
    predictor_filename = 'binning.csv')
```

### Read CDH Sample data

The library ships with data from the CDH Sample application. You can import it using the following lines:

```python
from cdhtools import datasets
CDHSample = datasets.CDHSample()
```

In the background, this imports the CDH Sample dataset from the GitHub repo directly and initializes them in an ADMDatamart class. 

### The data

Default datamart dumps contain data about the models and about the predictor binning. These are automatically detected and imported by the ADMDatamart class, so once the class is initialised you can start working directly. 

To access the raw data as imported by the ADMDatamart class as dataframes, simply call the .modelData or .predictorData properties of the initiated classes. If both model data as well as predictor data is supplied, a merge is done in the background automatically, which can also be accessed by calling .combinedData directly.

There are two main ways to subset the data: by calling the `last()` function or by supplying the `query` argument to any plotting function. The `last()` function subsets the data to only retain the last known snapshot for each model. The `query` argument applies the powerful [pandas querying functionality](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.query.html) to the data before plotting. See the Example ADM Analysis for further instructions. 

### Plots

There are a number of plotting functions included out of the box, an up-to-date overview of these plots can be found in the Graph Gallery.

It is possible to use "query" parameter in all of the methods to filter various values for better/detailed visualizations. By default, the plots are generated in Plotly - if you want to plot them using matplotlib, you can supply the 'plotting_engine = "mpl"' argument to any visualisation. Alternatively you may supply 'return_df = True' to any plotting function to retrieve the subsetted data that is used by the plots, so you can create the plots yourself.

To create a simple Bubble Chart (similar to the one in product):

```python
CDHSample.plotPerformanceSuccessRateBubbleChart()
```

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/gettingstartedPythonplot1.png" width="50%">

Or to create an overview of the predictor performance across all models:

```python
CDHSample.plotPredictorPerformance()
```

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/gettingstartedPythonplot2.png" width="50%">


## Reference and documentation
A reference to the functions available in the Python package is also available here: [Function Reference](https://pegasystems.github.io/cdh-datascientist-tools/Python/autoapi/index.html).

## Contributing

See [Contributing](Contributing#contributing-to-python)


