Analyze the performance of your models, over time, across channels, issues and groups using data from the ADM Datamart.

# Retrieving the data

To retrieve the data, follow one of these approaches

## Approach 1: Dataset export from Pega dev studio

From Pega Dev Studio, locate the dataset "pyModelSnapshots" in the Data-Decision-ADM-ModelSnapshot class. This dataset represents a view on the ADM Datamart table with the model snapshots. Export all data from this dataset by clicking "Export" from the "Actions" menu.

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/pega_export_adm_models.png" width="50%">

In the dialog that follows, press "Export". Depending on the size of your data, this may take a while. Then before dismissing the dialog, export the data from the Pega system by clicking the download link that will be shown when the export process has finished.

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/pega_export_dialog.png" width="50%">

The data will be stored in the download location of your browser in the standard Pega dataset export format: zipped, multi-line JSON. You can unzip and load this manually, but we have some utilities in `cdhtools` that make this easier for you.

### R

In the `cdhtools` library, use the [ADMDatamart](https://pegasystems.github.io/cdh-datascientist-tools/reference/ADMDatamart.html) function to load the ADM Datamart data. This function reads data, drops Pega-internal fields, standardizes the field names and performs other cleanup activities. In addition to dataset exports it can also read CSV, parquet and many other formats.

There also is a generic method to read any dataset (and which will not perform any of these cleanup activities): [readDSExport](https://pegasystems.github.io/cdh-datascientist-tools/reference/readDSExport.html).

In both functions you can omit the timestamp of the Pega file and it will always take the latest version of the file in the specified location. This is very convenient when you do multiple exports from Pega, so it always takes the latest export.

```r
dm <- ADMDatamart(folder = "~/Downloads")
```

### Python

For Python use the files from the GitHub repository directly. There is a utility function `readDSExport` in `cdh_utils.py` in the python folder.

```python
from ADMDatamart import ADMDatamart
dm = ADMDatamart("/data")
```

## Approach 2: Manual table export from database

The table with the model snapshots is `PR_DATA_DM_DATAMART_MDL_FACT`. You can export this using your favourite database tool. Optionally leave out Pega internal fields (starting with pz/px) and the (large) raw model data field (pymodeldata). 

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/pega_db_models.png" width="50%">

When exporting as a CSV be careful:
* Include a header with the names
* Make sure the column separator does not interfere with characters in the fields - a comma is not safe, the pipe character | is often a better choice
* If possible use double quotes around symbolic values

Then read the resulting file into R or Python and go from there. Date/time fields (only pySnapshotTime) really matters often needs attention when reading the CSV. The R ADMDatamart function has options for preprocessing in which this can be specified.

# Example analysis

## R

Now the data is retrieved, it is easy to create plots. The library provides several plots (see plot* functions in the [help](https://pegasystems.github.io/cdh-datascientist-tools/reference/index.html)), although it is easy enough to construct your own (see source of [plots.R](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/r/R/plots.R) for inspiration).

```r
library(cdhtools)
library(data.table)
library(ggplot2)
library(colorspace)

plotPerformanceSuccessRateBubbleChart(dm, facets = c("Channel","Issue")) +
   scale_color_discrete_divergingx()
```
<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/datamartplot1.png" width="50%">

# Bringing it all together

All the model analysis plots shown in the [gallery](CDH-Graph-Gallery) can be created using the sample code from the provided notebooks.

## R

Many of the plots can be re-created using the data provided with **cdhtools**. Using the R package this data is available when the `cdhtools` library is loaded. The raw data files (dataset exports and .csv files) are also available in the /extra folder of the repository. Some plots required such amounts of data that we did not want to include it in the repository. You can still run the code examples and get similar looking plots.

## Python

See the example notebook [Example_ADM_Analysis.ipynb](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/examples/datamart/Example_ADM_Analysis.ipynb)

## R

See the example notebook [adm-datamart.Rmd](https://pegasystems.github.io/cdh-datascientist-tools/articles/adm-datamart.html)

or when you have `cdhtools` installed, check the vignette `adm-datamart`: 

```r
library(cdhtools)

vignette("adm-datamart")
```




