# Welcome to the Customer Decision Hub Data Scientist Tools Wiki!

These open sourced accompanying tools to Pega CDH (Customer Decision Hub) provide Data Scientists tools to analyze analytical data from a Pega Decisioning system.

Tooling is both in R and Python although currently not everything is available in both languages. The current functionality includes

* Functions to read in data from the ADM Datamart, from Pega datasets and to easily create plots. [R Function Reference](https://pegasystems.github.io/cdh-datascientist-tools/reference/index.html) 
* Collection of sample plots/graphs for analysis of CDH. See our [CDH Graph Gallery](CDH-Graph-Gallery).
* An [all-in-one "Health Check" notebook](ADM-Datamart-Health-Check) that can be applied to the ADM Datamart to get insights into the models and predictors.
* Create stand-alone and off-line browsable Adaptive model reports in HTML or PDF. [Step-by-step tutorial to create offline model reports](Create-stand-alone-Adaptive-Model-Reports).
* Example analysis of the ADM Datamart: [R](https://pegasystems.github.io/cdh-datascientist-tools/articles/adm-datamart.html) [Python](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/examples/datamart/Example_ADM_Analysis.ipynb). For instructions on how to export the data, see [How to export and use the ADM Datamart](How-to-export-and-use-the-ADM-Datamart).
* Example analysis of the Historical Dataset: [R](https://pegasystems.github.io/cdh-datascientist-tools/articles/historical-dataset.html) [Python](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/examples/ih/Example_IH_Analysis.ipynb).
* Example of a Value Finder analysis: [R](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/examples/valuefinder/vf_analysis.Rmd) [Python](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/examples/valuefinder/vf_analysis.ipynb)
* Freeze Adaptive models into PMML Scorecards including reason codes for decision explanations. [Step-by-step tutorial to create PMML Scorecards from ADM](Create-PMML-from-ADM-Models)
* Restore Adaptive models from model snapshots (when "full auditability" is switched on). [Example notebook](https://pegasystems.github.io/cdh-datascientist-tools/articles/snapshots-to-scorecards.html)
* More examples in the [R vignettes](https://pegasystems.github.io/cdh-datascientist-tools/articles/index.html) and as [Python Jupyter notebooks](https://github.com/pegasystems/cdh-datascientist-tools/tree/master/examples).

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

4. Then load the `devtools` library and install the `cdhtools` package. Note the `build_vignettes` flag. When prompted for updates, we recommend installing only binary CRAN packages. Note that we have seen failures when installing over VPN, so you may have to (temporarily) go off VPN to install the package from GitHub.

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


To run the R examples you do not need to clone [the repository](https://github.com/pegasystems/cdh-datascientist-tools), but for the Python examples you do, as well as for some of the example files.


## Contents

The R package currently contains 

- Utilities to read data into `data.table` structures. The `ADMDatamart` function is the main work horse here, this reads downloaded Pega dataset exports as well as many other formats.
- Standard functions to easily create plots from Pega data, primarily from the ADM datamart. These plot functions can be further customized.
- Standard notebooks to generate off-line viewable reports that can be saved as stand-alone HTML or PDF files. There is one that strings together many of the plot functions to generate one big "datamart scan" including a lot of useful analyses. Another one creates a stand-alone model report similar to the view in Pega Prediction Studio, with all the predictor binning, the model classifier etc. These notebooks can easily be applied on any ADM datamart export.
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
- SKLearn

### Optional dependencies
- Pyarrow for faster data loading
- Plotly for some interactive charts

## Compatibility
The current version of the Python files is tested on Python version 3.10.2, but all scripts should work from version 3.6 on.

 
## Installation

With version 2.0.0, you can install CDH tools directly from within your environment with pip by running the following code:
```python
pip3 install git+https://github.com/pegasystems/cdh-datascientist-tools.git
```
Note: if you run this within a Jupyter cell be sure to add an exclamation point in front of the command.

## Read CDH Sample data

The library is shipped with a default dataset from the CDH Sample application. You can import it using the following lines after installing:

```python
from cdhtools import datasets
CDHSample = datasets.CDHSample()
```

In the background, this imports the CDH Sample dataset from the GitHub repo directly and initializes them in an ADMDatamart class. 

## Reading your own data
Of course, it is also possible to read your own data. Simply find the location of the data and supply it as the 'path' argument in the ADMDatamart class as below:

```python
from cdhtools import ADMDatamart
datamart = ADMDatamart(path='your-data-location')
```

Default datamart dumps contain data about the models and about the predictor binning. These are automatically detected and imported by the ADMDatamart class, so once the class is initialised you can start working directly. 

Note: if the model data and predictor data files have been renamed, they might not be detected automatically. In that case, you would need to supply their names to the class as such:

```python
datamart = ADMDatamart("path/datamart",
   model_filename = 'models.csv',
   predictor_filename = 'binning.csv')
```
This syntax also works for subdirectories: if models.csv is in a subfolder within the datamart folder named 'modeldatafile', the argument for model_filename would be 'modeldatafile/models.csv'.

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

Now you can call the methods within these classes to generate various graphs. It is possible to use "query" parameter in all of the methods to filter various values for better/detailed visualizations. By default, the plots are generated in Plotly - if you want to plot them using matplotlib, you can supply the 'plotting_engine = "mpl"' argument to any visualisation. Alternatively you may supply 'return_df = True' to any plotting function to retrieve the subsetted data that is used by the plots, so you can create the plots yourself.

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


Refer to `Example_ADM_Analysis.ipynb` file for a thorough example on how to use these two classes.


## To analyze IH data

Use `IHanalysis.py` to get insight into Interaction History (IH) data. This python file contains various methods each one providing certain visibility into the data. Simply import the IH data as a pandas dataframe into the jupyter file, then use various methods. An example is provided: `Example_IH_Analysis.ipynb`
