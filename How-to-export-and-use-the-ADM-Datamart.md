Analyze the performance of your models, over time, across channels, issues and groups using data from the ADM Datamart.

# Retrieving the data

To retrieve the data, follow one of these approaches

## Option 1: Dataset export from Pega dev studio

From Pega Dev Studio, locate the dataset "pyModelSnapshots" in the Data-Decision-ADM-ModelSnapshot class. This dataset represents a view on the ADM Datamart table with the model snapshots. Export all data from this dataset by clicking "Export" from the "Actions" menu.

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/pega_export_adm_models.png" width="50%">

In the dialog that follows, press "Export". Depending on the size of your data, this may take a while. Then before dismissing the dialog, export the data from the Pega system by clicking the download link that will be shown when the export process has finished.

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/pega_export_dialog.png" width="50%">

The data will be stored in the download location of your browser in the standard Pega dataset export format: zipped, multi-line JSON. You can unzip and load this manually, but we have some utilities in `cdhtools` that make this easier for you.

Repeat the steps for the predictor data, which is stored in a separate table. You typically want both tables, although this is not mandatory. Many of the standard plot functions will require both to be present.

|Data|Class|Dataset|Table|
|---|---|---|---|
|Model Snapshots|Data-Decision-ADM-ModelSnapshot|pyModelSnapshots|PR_DATA_DM_ADMMART_MDL_FACT|
|Predictor Snapshots|Data-Decision-ADM-PredictorBinningSnapshot|pyADMPredictorSnapshots|PR_DATA_DM_ADMMART_PRED|

See (https://docs.pega.com/decision-management-reference-materials/database-tables-monitoring-models) for more information.

### R

In the `cdhtools` library, use the [ADMDatamart](https://pegasystems.github.io/cdh-datascientist-tools/reference/ADMDatamart.html) function to load the ADM Datamart data. This function reads data, drops Pega-internal fields, standardizes the field names and performs other cleanup activities. In addition to dataset exports it can also read CSV, parquet and many other formats.

There also is a generic method to read any dataset (and which will not perform any of these cleanup activities): [readDSExport](https://pegasystems.github.io/cdh-datascientist-tools/reference/readDSExport.html).

In both functions you can omit the timestamp of the Pega file and it will always take the latest version of the file in the specified location. This is very convenient when you do multiple exports from Pega, so it always takes the latest export.

```r
library(cdhtools)
dm <- ADMDatamart(folder = "~/Downloads")
```

### Python

For Python use the files from the GitHub repository directly. Use the `ADMDatamart` class to read the data. For advanced use there is a utility function `readDSExport` in `cdh_utils.py`.

```python
from ADMDatamart import ADMDatamart
dm = ADMDatamart("~/Downloads")
```

## Option 2: Export only selected models from Pega

This is like the first option, however now instead of including the full model / predictor tables you only select the models and predictors that you are interested in.

1. Create a dataflow on **Data-Decision-ADM-ModelSnapshot**
2. Source the dataflow with the **pyModelSnapshots** dataset
3. Insert a Filter shape after the source dataset to filter on the models of interest. If you filter by rule that would be a condition on **.pyConfigurationName**.
4. Create a Cassandra dataset as the destination. The keys the system shows when saving it (model ID, snapshot time, application) are fine.

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/DataFlowExportSelectedModels.png" width="100%">

That's it for the model data. Run this dataflow and export the destination dataset, then follow the steps from option 1 to load them in your R or Python environment.

For the predictor data you can follow the exact same pattern. However that would require you to know the ModelID's, so it is preferable to piggy-back on the model data you exported and let the system figure out which ModelID's to use.

1. Create a dataflow on **Data-Decision-ADM-PredictorBinningSnapshot**
2. Source the predictor flow with the **pyADMPredictorSnapshots** dataset
3. Instead of a filter like in the previous one, first add a Compose shape
4. Go back to your model data flow and make the destination abstract. Then use this in the Compose. The condition is equal **pyModelID**, use a new property to hold the model data (single page property of class **Data-Decision-ADM-ModelSnapshot**).
5. Add a filter to filter out any predictor data that does not match
6. Destination is a custom Cassandra dataset like before

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/DataFlowExportSelectedPredictors.png" width="100%">

Run this dataflow and export the destination dataset, then follow the steps from option 1 to load them in your R or Python environment.

### R

```r
library(cdhtools)
dm <- ADMDatamart("Data-Decision-ADM-ModelSnapshot_MyModelSnapshots", 
                  "Data-Decision-ADM-PredictorBinningSnapshot_MyPredictorSnapshots",
                  folder = "~/Downloads")
```

### Python

```python
from ADMDatamart import ADMDatamart
dm = ADMDatamart("Data-Decision-ADM-ModelSnapshot_MyModelSnapshots", 
                 "Data-Decision-ADM-PredictorBinningSnapshot_MyPredictorSnapshots",
                 folder = "~/Downloads")
```

## Option 3: Manual table export from database

The table with the model snapshots is `PR_DATA_DM_DATAMART_MDL_FACT`. You can export this using your favourite database tool. Optionally leave out Pega internal fields (starting with pz/px) and the (large) raw model data field (pymodeldata). 

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/pega_db_models.png" width="50%">

When exporting as a CSV be careful:
* Include a header with the names
* Make sure the column separator does not interfere with characters in the fields - a comma is not safe, the pipe character | is often a better choice
* If possible use double quotes around symbolic values
* If possible use an unambigous standard format for the date/time fields (pySnapshotTime)

Then read the resulting file into R or Python and go from there. Reading CSV's with embedded comma's can be challenging. The R ADMDatamart function has options for preprocessing where clean-up actions can be specified. If that does not provide enough flexibility read the data first, then pass it in as data objects into the ADMDatamart class/function.

### R

```r
dm <- ADMDatamart("models.csv", "preds.csv", folder="adm")
```

### Python

```python
dm <- ADMDatamart("models.csv", "preds.csv", folder="adm")
```

# Visualisation of the Datamart

Now the data is retrieved, it is easy to create plots and perform all sorts of analyses. The library provides several standard plots (see plot* functions in the [help](https://pegasystems.github.io/cdh-datascientist-tools/reference/index.html)), although it is easy enough to construct your own (see source of the plot functions in the repository).

## R

```r
plotPerformanceSuccessRateBubbleChart(dm)
```

## Python

```python
dm.plotPerformanceSuccesRateBubbleChart()
```

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/datamartplot1.png" width="50%">
