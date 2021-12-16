Analyze the performance of your models, over time, across channels, issues and groups using data from the ADM Datamart.

# Retrieving the data

To retrieve the data, follow one of these approaches

## Approach 1: Dataset export from Pega dev studio

From Pega Dev Studio, locate the dataset "pyModelSnapshots". This dataset represents a view on the ADM Datamart table with the model snapshots. Export all data from this dataset by clicking "Export" from the "Actions" menu.

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/pega_export_adm_models.png" width="50%">

In the dialog that follows, press "Export". Depending on the size of your data, this may take a while. Then before dismissing the dialog, export the data from the Pega system by clicking the download link that will be shown when the export process has finished.

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/pega_export_dialog.png" width="50%">

The data will be stored in the download location of your browser in the standard Pega dataset export format: zipped, multi-line JSON. You can unzip and load this manually, but we have some utilities in `cdhtools` that make this easier for you.

### R

In the `cdhtools` library, there is a generic method to read dataset exports into a `data.table`: [readDSExport](https://pegasystems.github.io/cdh-datascientist-tools/reference/readDSExport.html). There also is a convenience wrapper [readADMDatamartModelExport](https://pegasystems.github.io/cdh-datascientist-tools/reference/readADMDatamartModelExport.html) that is aware of the standard name of the export file (e.g. _Data-Decision-ADM-ModelSnapshot_pyModelSnapshots_20201215T093542_GMT.zip_), leaves out Pega internal fields and that maps date/time fields to appropriate R types. Both these functions by default ignore the date/time part of the file and take the latest version of the file in the specified location. This is very convenient when you do multiple exports from Pega, the script will always take the latest export.

By default it takes all snapshots, you can specify a flag `latestOnly` to only take the latest snapshots of each model. Alternatively you can do this in R (in `data.table` syntax: `models[, .SD[which.max(SnapshotTime)], by=ModelID]`). 

```r
models <- readADMDatamartModelExport(srcFolder = "~/Downloads")
```

### Python

For Python use the files from the GitHub repository directly. There is a utility function `readDSExport` in `cdh_utils.py` in the python folder.

```python
models = readDSExport("Data-Decision-ADM-ModelSnapshot_pyModelSnapshots", "~/Downloads")
```

## Approach 2: Manual table export from database

The table with the model snapshots is `PR_DATA_DM_DATAMART_MDL_FACT`. You can export this using your favourite database tool. Optionally leave out Pega internal fields (starting with pz/px) and the raw model data field (pymodeldata). 

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/pega_db_models.png" width="50%">

Then read the resulting file into R or Python and go from there. Just take care of the format of e.g. data/time fields in the export from the DB tool.

## Approach 3: Table export using cdhtools

The `cdhtools` library can also do the database export for you and format the data in the desired format.

Given a `Connection`, the function [readADMDatamartModelTable](https://pegasystems.github.io/cdh-datascientist-tools/reference/readADMDatamartModelTable.html) will fetch the data for you and return a `data.table` in the same way the dataset read function is doing.

```r
library(cdhtools)
library(data.table)
library(RJDBC)

drv <- JDBC("org.postgresql.Driver", "<LOCATION OF YOUR DRIVER")
pg_host <- "<HOST>:5432"
pg_db <- "<DB NAME>"
pg_user <- "<DB USER>"
pg_pwd <- "<DB PASSWORD>"

conn <- dbConnect(drv, paste("jdbc:postgresql://", pg_host,  "/", pg_db, sep=""), pg_user, pg_pwd)
models <- getModelsFromDatamart(conn)
```

The `getModelsFromDatamart` has options to select models for only certain applications, configurations etc.

# Example analysis

## R

Now the data is retrieved, it is easy to create plots. The library provides several plots (see plotADM* functions in the [help](https://pegasystems.github.io/cdh-datascientist-tools/reference/index.html)), although it is easy enough to construct your own (see source of [plots.R](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/r/R/plots.R) for inspiration).

```r
library(cdhtools)
library(data.table)
library(ggplot2)
library(colorspace)

plotADMPerformanceSuccessRateBubbleChart(models, facets = c("Channel","Issue")) +
   scale_color_discrete_divergingx()
```
<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/datamartplot1.png" width="50%">

## Python

# Bringing it all together

All the model analysis plots shown in the [gallery](CDH-Graph-Gallery) can be created using the sample codes from the provided notebooks.

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




