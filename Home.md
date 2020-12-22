# Welcome to the Customer Decision Hub Data Scientist Tools Wiki!

These open sourced accompanying tools to Pega CDH (Customer Decision Hub) provide data scientists some tooling to analyze the data produced by a running Pega Decisioning system. It includes tools to easily read Dataset exports, pull data from ADM or IH, create off-line browsable Adaptive Model reports and more. 

Tooling is both in R and Python although currently not everything is available in both languages. 

# CDH Graph Gallery

Examples of useful graphs to get insights into CDH using data from the Pega platform.

[CDH Graph Gallery](CDH-Graph-Gallery)

## Getting Started with the R library

The **cdhtools** package can be installed straight from GitHub and provides a number of utility functions and demo scripts.

First, make sure you have [RStudio](https://rstudio.com/products/rstudio/) installed. To run the R examples you do not need to clone [the repository](https://github.com/pegasystems/cdh-datascientist-tools), but for the Python examples you do. Also, if you want to access some of the example files you will need to clone [the repository](https://github.com/pegasystems/cdh-datascientist-tools).

To install the package use the `devtools` package. If you don't have that installed yet, do that first:

```r
install.packages("devtools")
```

Then load the `devtools` library and install the `cdhtools` package. Note the `build_vignettes` flag.

```r
library(devtools)
install_github("pegasystems/cdh-datascientist-tools/r", build_vignettes=TRUE)
```

If all is well, this will then install an R package called `cdhtools` that you can then use just like any other R package. The package contains help and vignettes to help you get going. You can quickly check this by running the following R commands:

```r
library(cdhtools)

?cdhtools
browseVignettes("cdhtools")
vignette(topic="adm-datamart")
```

Which should show you an overview of the vignettes (package help), opens one of them and shows generic package help. 


### Content

The R package currently contains 

- Some utilities to make it easier to work with Pega, like reading the dataset exports into `data.table` structures. The `readDSExport` function is the main work horse here, this reads downloaded Pega dataset exports. Specialized versions of this read data from ADM or IH exports.
- Standard notebooks to generate off-line viewable, stand-alone model reports and a model overview. These reports are similar to the reports in the product but can be generated and browsed off-line, but they also add some functionality not currently present in the product, like showing the active bins of the propensity mapping, an overview of predictor performance across models in the form of boxplots, and some more. They are parameterized and can easily be applied to any export of the ADM datamart.
- An (experimental) utility to take an ADM model and transform it into PMML. This PMML is basically a "frozen" version of the ADM model with each model instance represented as as Score Card including reason codes that can be used to explain the decision.


The available vignettes are (`vignette(package="cdhtools")`):

Vignette | Description | Read with
------------ | ------------- | -------------
adhoc-datasetanalysis | Using Dataset Exports | `vignette("adhoc-datasetanalysis")`
adm-datamart | Reporting on the ADM Datamart | `vignette("adm-datamart")`
adm-explained | Detailed explanation with formulas and code of the ADM Model Reports | `vignette("adm-explained")`
ih-reporting | Reporting on Interaction History | `vignette("ih-reporting")`


You can get the list of vignettes with `browseVignettes("cdhtools")` (as a web page) or `vignette(package="cdhtools")`. A vignette provides the original source as well as a readable HTML or PDF page and a file with the R code. Read a specific one with `vignette(x)` and see its code with `edit(vignette(x))`.

The other option is to download the source (clone from [the GitHub repository](https://github.com/pegasystems/cdh-datascientist-tools)) and use the functions and demo scripts directly. The R code, tests, vignettes etc are in the `r` subdirectory.

## R Reference

[ADM Datamart Examples](https://pegasystems.github.io/cdh-datascientist-tools/articles/adm-datamart.html)

[Working with Dataset exports](https://pegasystems.github.io/cdh-datascientist-tools/articles/adhoc-datasetanalysis.html)

[Function Reference](https://pegasystems.github.io/cdh-datascientist-tools/reference/index.html)

Notebooks for Model and Predictor reports in the "extra" folder that extend product capabilities.


## Getting Started with the Python notebooks

The Python part of the tools currently contain a subset of the functionality provided by the R version:

- A port of the `cdh_utils` module with utilities to e.g. easily read a Pega dataset export.
- Two classes in `model_report.py` to generate reports from the ADM datamart. 
- A number of methods within `IHanalysis.py` file to get insight into interaction history data

The Python code does not build a package/library so to use it clone the github repository. To import the module type

```python
import cdh_utils as cu
```

Then, run the different functions in this module.

For example, for the readDSExport function:

```python
df1 = cu.readDSExport("Data-Decision-ADM-ModelSnapshot_AllModelSnapshots", srcFolder="inst/extdata", tmpFolder="tmp")
df2 = cu.readDSExport("Data-Decision-ADM-ModelSnapshot_AllModelSnapshots_20180316T134315_GMT.zip", srcFolder="inst/extdata", tmpFolder="tmp3")
```

### To analyze ADM datamart in python:

Two classes need to be instantiated. One for the model report and one for the predictor report. 
Once df1 and df2 as described above are imported, use the following example to instantiate your classes:

```python
from model_report import ADMReport, ModelReport

Models = ModelReport(np.array(df1['pyModelID']), np.array(df1['pyName']), 
                     np.array(df1['pyPositives']), np.array(df1['pyResponseCount']), 
                     np.array(df1['pyPerformance']), np.array(df1['pySnapshotTime']))
                     
Preds = ADMReport(Models.modelID, Models.modelName, Models.positives, Models.responses, Models.modelAUC, 
                  Models.modelSnapshot, np.array(df2['pyModelID']), np.array(df2['pyPredictorName']), 
                  np.array(df2['pyPerformance']), np.array(df2['pyBinSymbol']), 
                  np.array(df2['pyBinIndex']), np.array(df2['pyEntryType']), 
                  np.array(df2['pyType']), np.array(df2['pySnapshotTime']), 
                  np.array(df2['pyBinPositives']), np.array(df2['pyBinResponseCount']))
```
       
Call the methods within these classes to generate various graphs. It is possible to use "query" parameter in most of the methods to filter various values for better/detailed visualizations.

Refer to `Example_ADM_Analysis.ipynb` file for a thorough example on how to use these two classes.


### To analyze IH data

Use `IHanalysis.py` to get insight into Interaction History (IH) data. This python file contains various methods each one providing certain visibility into the data. Simply import the IH data as a pandas dataframe into the jupyter file, then use various methods. An example is provided: `Example_IH_Analysis.ipynb`


## Getting Started with the Java utils

Included is an experimental tool to work with internal Cassandra.

To build it, run `gradle allinone` in java/DDSUtil. To run, `java -jar java/DDSUtil/build/libs/DDSUtil-1.0-SNAPSHOT.jar`. The tool provides help.

