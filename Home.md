# Welcome to the Customer Decision Hub Data Scientist Tools Wiki!

These open sourced accompanying tools to Pega CDH (Customer Decision Hub) provide data scientists some tooling to analyze the data produced by a Pega Decisioning system.

Tooling is both in R and Python although currently not everything is available in both languages. The current functionality includes

* Easily read in data from Pega Dataset exports, both generically and specifically for the datamart data. [Function Reference](https://pegasystems.github.io/cdh-datascientist-tools/reference/index.html) 
* Collection of standard plots/graphs for analysis of CDH. See our [CDH Graph Gallery](CDH-Graph-Gallery).
* Off-line browsable Adaptive model reports. [Step-by-step tutorial to create offline model reports](Create-offline-Adaptive-Model-Reports).
* Example of analysis of the ADM Datamart: [R](https://pegasystems.github.io/cdh-datascientist-tools/articles/adm-datamart.html) [Python](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/examples/datamart/Example_ADM_Analysis.ipynb).
* Example of analysis of the Historical Dataset: [R](https://pegasystems.github.io/cdh-datascientist-tools/articles/historical-dataset.html) [Python](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/examples/ih/Example_IH_Analysis.ipynb).
* Example of a Value Finder analysis: [Python](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/examples/valuefinder/vf_analysis.ipynb)
* More examples in the R vignettes (https://pegasystems.github.io/cdh-datascientist-tools/articles/index.html) and as Python Jupyter notebooks (https://github.com/pegasystems/cdh-datascientist-tools/tree/master/python).
* Export Adaptive models to PMML. [Step-by-step tutorial to create PMML Scorecards from ADM](Create-PMML-from-ADM-Models)
* Restore Adaptive models from model snapshots (when "full auditability" is switched on). [Example notebook](https://pegasystems.github.io/cdh-datascientist-tools/articles/snapshots-to-scorecards.html)


# Getting Started with the R library

The `cdhtools` package can be installed straight from GitHub. First you will need R and R Studio.

Installation steps:

1. Install R
    If you do not have R installed, go to https://www.r-project.org/ and install the latest version of the software. Find the correct installer for your platform (e.g. R-4.0.3.pkg) and follow the steps of the installer.

2. Install R Studio
    Install R Studio from [rstudio.com](https://rstudio.com/products/rstudio/). Follow the installation steps, then launch R Studio. On first launch, it should find R automatically, otherwise you will need to configure. We don't recommend installing with Homebrew (Mac) because this seems to default all packages to install from source which can cause trouble.

3. To install `cdhtools` from GitHub use the `devtools` package. If you don't have that installed yet, do that first:

```r
install.packages("devtools")
```

4. Then load the `devtools` library and install the `cdhtools` package. Note the `build_vignettes` flag. When prompted for updates, we recommend installing only binary CRAN packages.

```r
library(devtools)
install_github("pegasystems/cdh-datascientist-tools/r", build_vignettes=TRUE)
```

5. If all is well, this will install an R package called `cdhtools` that you can then use just like any other R package. The package contains help and vignettes to help you get going, both also accessible from the Github page directly. You can quickly check this by running the following R commands. This should show you an overview of the vignettes (package help), opens one of them and shows generic package help. 

```r
library(cdhtools)

?cdhtools
browseVignettes("cdhtools")
vignette(topic="adm-datamart")
```

6. It ships with some data as well. Create your first plots with just a few lines:

```r
library(cdhtools)
library(ggplot2)
library(data.table)

data("admdatamart_models")

plotADMPerformanceSuccessRateBubbleChart(admdatamart_models)
```
<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/gettingstartedRplot1.png" width="50%">

You can add standard `ggplot` decoration to the returned plots, and most of the plot functions also have parameters to select the fields to aggregate or facet by, see help.

To get a global overview of the predictor performance in all the models:

```r
data("admdatamart_binning")

plotADMPredictorPerformance(admdatamart_binning)
```

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/gettingstartedRplot2.png" width="50%">


To run the R examples you do not need to clone [the repository](https://github.com/pegasystems/cdh-datascientist-tools), but for the Python examples you do, as well as for some of the example files.


## Contents

The R package currently contains 

- Some utilities to make it easier to work with Pega, like reading the dataset exports into `data.table` structures. The `readDSExport` function is the main work horse here, this reads downloaded Pega dataset exports. Specialized versions of this read data from ADM or IH exports.
- Standard notebooks to generate off-line viewable, stand-alone model reports and a model overview. These reports are similar to the reports in the product but can be generated and browsed off-line, but they also add some functionality not currently present in the product, like showing the active bins of the propensity mapping, an overview of predictor performance across models in the form of boxplots, and some more. They are parameterized and can easily be applied to any export of the ADM datamart.
- An (experimental) utility to take an ADM model and transform it into PMML. This PMML is basically a "frozen" version of the ADM model with each model instance represented as as Score Card including reason codes that can be used to explain the decision.

## Reference

The available vignettes are (`vignette(package="cdhtools")`):

Vignette | Description | Read with
------------ | ------------- | -------------
adhoc-datasetanalysis | Using Dataset Exports | `vignette("adhoc-datasetanalysis")`
adm-datamart | Reporting on the ADM Datamart | `vignette("adm-datamart")`
adm-explained | Step by step explanation of how ADM calculates propensities | `vignette("adm-explained")`
ih-reporting | Reporting on Interaction History | `vignette("ih-reporting")`
snapshots-to-scorecards | Example code to turn a model snapshot into a scorecard | `vignette("snapshots-to-scorecards")`

You can get the list of vignettes with `browseVignettes("cdhtools")` (as a web page) or `vignette(package="cdhtools")`. A vignette provides the original source as well as a readable HTML or PDF page and a file with the R code. Read a specific one with `vignette(x)` and see its code with `edit(vignette(x))`.

The other option is to download the source (clone from [the GitHub repository](https://github.com/pegasystems/cdh-datascientist-tools)) and use the functions and demo scripts directly. The R code, tests, vignettes etc are in the `r` subdirectory.

A reference to the functions available in the R package is also available here: [Function Reference](https://pegasystems.github.io/cdh-datascientist-tools/reference/index.html).


# Getting Started with the Python notebooks

The Python part of the tools currently contain a subset of the functionality provided by the R version:

- A port of the `cdh_utils` module with utilities to e.g. easily read a Pega dataset export.
- A `ADMDatamart` class in the `ADMDatamart.py` file for preprocessing of datamart dumps
- A `ADMVisualisations` class in the `plots.py` file for visualisation of datamart dumps
- A number of methods within `IHanalysis.py` file to get insight into interaction history data

The Python code does not build a package/library so to use it clone the github repository. 

## Dependencies
- Pandas
- Numpy
- Matplotlib
- Seaborn

### Optional dependencies
- Pyarrow for faster data loading
- Plotly for some interactive charts

## Compatibility
The current version of the Python files is tested on Python version 3.8, but all scripts should work from version 3.6 on.

Launch Jupyter and create a Python 3 script in the "python" folder of the checked out repository. Start
with the import of the utilities.
 
## Quickstart

To read a single file, use readDSExport from cdh_utils:

```python
from cdh_utils import readDSExport
data = readDSExport(path = "/data", file = "datafile.zip")
```

## Read ADM data

There are two default datamart dump files included with the repository, which you can import with the ADMDatamart class, simply by initialising it with the folder of the datamart dump as the argument.

```python
CDHSample = ADMDatamart("../../data")
```

The default datamart dump contains data about the models and about the predictor binning. These are automatically detected and imported by the ADMDatamart class, so once the class is initialised you can start working directly.

## Accessing the raw data 

To access the raw data as imported by the ADMDatamart class as dataframes, simply call the .modelData or .predictorData properties of the initiated classes. If both model data as well as predictor data is supplied, a merge is done in the background automatically, which can also be accessed by calling .combinedData directly.

## Plots

There are a number of plotting functions included out of the box, an overview of which can be found below: 

| Visualisation                            | Needs model data | Needs multiple snapshots | Needs predictor data |
|------------------------------------------|------------------|--------------------------|----------------------|
| plotPerformanceSuccessRateBubbleChart    | True             | False                    | False                |
| plotPerformanceAndSuccessRateOverTime    | True             | True                     | False                |
| plotResponseCountMatrix                  | True             | True                     | False                |
| plotSuccessRateOverTime                  | True             | True                     | False                |
| plotPropositionSuccesRates               | True             | False                    | False                |
| plotScoreDistribution                    | True             | False                    | True                 |
| plotPredictorBinning                     | True             | False                    | True                 |
| plotPredictorPerformance                 | True             | False                    | True                 |
| plotPredictorPerformanceHeatmap          | True             | False                    | True                 |
| plotImpactInfluence                      | True             | False                    | True                 |

Now you can call the methods within these classes to generate various graphs. It is possible to use "query" parameter in all of the methods to filter various values for better/detailed visualizations.

To create a simple Bubble Chart (similar to the one in product):

```python
CDHSample.plotPerformanceSuccesRateBubbleChart()
```

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/gettingstartedPythonplot1.png" width="50%">

Or to create an overview of the predictor performance across all models:

```python
CDHSample.plotPredictorPerformance()
```

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/gettingstartedPythonplot2.png" width="50%">


Refer to `Example_ADM_Analysis.ipynb` file for a thorough example on how to use these two classes.


## To analyze IH data

Use `IHanalysis.py` to get insight into Interaction History (IH) data. This python file contains various methods each one providing certain visibility into the data. Simply import the IH data as a pandas dataframe into the jupyter file, then use various methods. An example is provided: `Example_IH_Analysis.ipynb`
