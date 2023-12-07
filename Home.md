# Welcome to the Pega Data Scientist Tools Wiki!

The Pega Data Science tools help to analyze the value and analytical performance of applications based on Pega AI, machine learning and decisioning, such as implementations of Pega Customer Decision Hub or Pega Process AI.

The tooling consists of scripts, applications and libraries and is Open Source, not an official Pega product. We release this under the Apache 2.0 license. Pega does not make any representation or warranty with respect to this free software. It targets data scientists as well as people with a less specialized role looking more for generic insights. Technically, most tooling is in Python but there is a legacy framework in R that we keep but are not actively developing anymore.

Functionality includes

* A standardized [ADM Health Check](ADM-Health-Check) to get insights into the models and predictors. This Health Check can be done from a data science environment with Python or R but can also, conveniently, be done from a stand-alone application. This application can generate stand-alone HTML ADM overview reports, as well as detailed reports for individual ADM models.
* Examples/starter scripts run your own, custom, ADM datamart analysis: [Python](https://pegasystems.github.io/pega-datascientist-tools/Python/articles/Example_ADM_Analysis.html) [R](https://pegasystems.github.io/pega-datascientist-tools/R/articles/adm-datamart.html).
* Example of a [Value Finder analysis](https://pegasystems.github.io/pega-datascientist-tools/Python/articles/vf_analysis.html) that supplements the built-in product features.
* Example analysis of the Historical Dataset that can be exported from ADM models in production: [R](https://pegasystems.github.io/pega-datascientist-tools/R/articles/historical-dataset.html) [Python](https://github.com/pegasystems/pega-datascientist-tools/blob/master/examples/ih/Example_IH_Analysis.ipynb).
* Explanatory articles on ADM, Thompson Sampling etc with working examples
* For the more technical, data science oriented users there are functions in [Python](https://pegasystems.github.io/pega-datascientist-tools/Python/autoapi/index.html) and [R](https://pegasystems.github.io/pega-datascientist-tools/R/reference/index.html) to work with Pega data, visualize it and more. 
* A collection of sample plots/graphs for analysis of CDH, more as inspiration. See our [Graph Gallery](Graph-Gallery).

Whilst the examples are CDH oriented, the code will generalize to analyzing other Pega AI applications, such as applications based on Process AI.

# Documentation and API reference

The [documentation](https://pegasystems.github.io/pega-datascientist-tools/) contains the API reference for both the [Python](https://pegasystems.github.io/pega-datascientist-tools/Python/index.html) and [R](https://pegasystems.github.io/pega-datascientist-tools/R/index.html) code as well as the examples and tutorials. 


# Getting Started with the Python tools

Pega Data Scientist Tools is intended to be used with **Python 3.8 and up**.

## Installation

```python
pip install pdstools
```
The tested library is uploaded to [pypi](https://pypi.org/project/pdstools/) on every release version. 
Note: If you run this within a Jupyter notebook cell, be sure to add an exclamation mark in front of the command.

To install the optional app dependencies type

`pip install --upgrade pdstools[app]`

For zsh on Mac use quotes: `pip install --upgrade pdstools'[app]'`


### Development branch
To instead install the development version, run the following command:
```python
pip install git+https://github.com/pegasystems/pega-datascientist-tools.git 
```

### Dependencies
- Pandas for the data structure
- Plotly >= 5.14.1 for the interactive visualisations
- Seaborn for the static visualisations
- Requests for importing data through a direct link
- Jupyterlab for jupyter notebook support
- Ipywidgets for interactive notebook widgets
- Pyarrow for faster data loading
- Polars for faster data processing

### Optional dependencies
- Dot for visualising ADM Trees

### Compatibility
The current version of the Python files is tested on Python versions 3.8 and 3.9 and 3.11. We recommend using plain notebooks or notebooks within VSCode's notebook editor.

## Using the Python tools

After installation, you can import the **ADMDatamart** class from the pdstools package as such:
```python
from pdstools import ADMDatamart
```
The ADMDatamart class is the main class which orchestrates reading, preprocessing and visualizing of the data. For an overview of its methods, please refer to the API reference. Normally, if the dataset is exported as per [the instructions page](https://github.com/pegasystems/pega-datascientist-tools/wiki/How-to-export-and-use-the-ADM-Datamart), the only path you would need to give the ADMDatamart class is the directory of that data, and it should find the filenames itself. If, for some reason, those are changed, you can of course supply those names with the 'model_filename' and 'predictor_filename' arguments. This syntax also works for subdirectories: if models.csv is in a subfolder within the datamart folder named 'modeldatafile', the argument for model_filename would be 'modeldatafile/models.csv'.


```python
simple_data = ADMDatamart("~/Downloads/CDHSample")
custom_file_names = ADMDatamart("~/Downloads/CDHSample",
    model_filename = 'models.csv',
    predictor_filename = 'binning.csv')
```

### Read CDH Sample data

The library ships with data from the CDH Sample application. You can import it using the following lines:

```python
from pdstools import datasets
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

<img src="/pegasystems/pega-datascientist-tools/blob/master/images/gettingstartedPythonplot1.png" width="50%">

Or to create an overview of the predictor performance across all models:

```python
CDHSample.plotPredictorPerformance(top_n=20)
```

<img src="/pegasystems/pega-datascientist-tools/blob/master/images/gettingstartedPythonplot2.png" width="80%">


## Reference and documentation
A reference to the functions available in the Python package is also available here: [Function Reference](https://pegasystems.github.io/pega-datascientist-tools/Python/autoapi/index.html).

## Contributing

See [Contributing](Contributing#contributing-to-python)

# Getting Started with the R library

## Installation

The `pdstools` package can be installed straight from GitHub. First you will need R and R Studio:

1. If you do not have **R** installed, go to https://www.r-project.org/ and install the latest version of the software. Find the correct installer for your platform (e.g. R-4.0.3.pkg) and follow the steps of the installer.

2. Install **R Studio** from [rstudio.com](https://rstudio.com/products/rstudio/). Follow the installation steps, then launch R Studio. On first launch, it should find R automatically, otherwise you will need to configure. We do not recommend installing with Homebrew (Mac) because this seems to default all packages to install from source which can cause trouble.

3. To install `pdstools` from GitHub use the `devtools` package. If you don't have that installed yet, do that first:

```r
install.packages("devtools")
```

4. Then load the `devtools` library and install the `pdstools` package. Note the `build_vignettes` flag. When prompted for updates, we recommend installing only binary CRAN packages. Note that we have seen failures when installing over VPN, so you may have to (temporarily) go off VPN to install the package from GitHub.

```r
library(devtools)
install_github("pegasystems/pega-datascientist-tools/r", build_vignettes=TRUE)
```

## Using the R package

If all is well, this will install an R package called `pdstools` that you can then use just like any other R package. The package contains help and vignettes to help you get going, both also accessible from the Github page directly. You can quickly check this by running the following R commands. This should show you an overview of the vignettes (package help), opens one of them and shows generic package help. 

```r
library(pdstools)

?pdstools
browseVignettes("pdstools")
vignette(topic="adm-datamart")
```

The package ships with some data. Create your first plots with just a few lines:

```r
library(pdstools)
library(ggplot2)
library(data.table)

data("adm_datamart")

plotPerformanceSuccessRateBubbleChart(adm_datamart)
```
<img src="/pegasystems/pega-datascientist-tools/blob/master/images/gettingstartedRplot1.png" width="50%">

You can add standard `ggplot` decoration to the returned plots, and most of the plot functions also have parameters to select the fields to aggregate or facet by, see help.

For example, to get an overview of the 20 most important predictors in all the models on a white background:

```r
plotPredictorImportance(adm_datamart, limit = 20) + theme_bw()
```

<img src="/pegasystems/pega-datascientist-tools/blob/master/images/gettingstartedRplot2.png" width="80%">


To run the R examples you do not need to clone [the repository](https://github.com/pegasystems/pega-datascientist-tools), but for some of the example files you do.


## Package Overview

The R package currently contains 

- Utilities to read data into `data.table` structures. The `ADMDatamart` function is the main work horse here, this reads downloaded Pega dataset exports as well as many other formats.
- Standard functions to easily create plots from Pega data, primarily from the ADM datamart. These plot functions can be further customized.
- Standard notebooks to generate off-line viewable reports that can be saved as stand-alone HTML or PDF files. There is one that strings together many of the plot functions to generate one big "datamart scan" including a lot of useful analyses. Another one creates a stand-alone model report similar to the view in Pega Prediction Studio, with all the predictor binning, the model classifier etc. These notebooks can easily be applied on any ADM datamart export.
- An (experimental) utility to take an ADM model and transform it into PMML. This PMML is basically a "frozen" version of the ADM model with each model instance represented as as Score Card including reason codes that can be used to explain the decision.

You can get the list of vignettes with `browseVignettes("pdstools")` (as a web page) or `vignette(package="pdstools")`. A vignette provides the original source as well as a readable HTML or PDF page and a file with the R code. Read a specific one with `vignette(x)` and see its code with `edit(vignette(x))`.

The other option is to download the source (clone from [the GitHub repository](https://github.com/pegasystems/pega-datascientist-tools)) and use the functions and demo scripts directly. The R code, tests, vignettes etc are in the `r` subdirectory.

A reference to the available functions is also published on GitHub: [Function Reference](https://pegasystems.github.io/pega-datascientist-tools/R/reference/index.html).

## Contributing

See [Contributing](Contributing#contributing-to-r)



