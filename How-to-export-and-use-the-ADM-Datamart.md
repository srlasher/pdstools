This article describes how to export the model datamart for the purposes of analysis of the performance of your models, over time, across channels, issues and groups.

The model datamart consists of two tables for ADM and another seven that support Predictive Models and Predictions. We focus on the ADM tables here as they contain detailed information not available in Prediction Studio. The data in the Predictive Models and Predictions tables surfaces directly in Prediction Studio, so off-line analysis of that is a less common use case.

The tables are described here [in the Pega documentation](https://docs.pega.com/decision-management-reference-materials/database-tables-monitoring-models).

There are three options to retrieve the data. We list them here in order of preference:

1. From Pega 8.8 onwards, you can export all the datamart tables into a repository directly from Prediction Studio. 

2. From Pega Dev Studio, export each of the tables via the Dataset wrappers that are defined on top of them. This is described in more detail below.

3. Export the tables directly from the database. If you do this, be careful with the output format. Treatments and actions can contain spaces and the pyName field can contain quotes. If you export directly from a database to CSV, make sure you choose CSV options so these special characters are preserved and can be read back in.

# Export from Prediction Studio

This is the easiest option, available in Pega 8.8 and up.

1. Set up a repository (to e.g. an S3 bucket). See [documentation](https://docs-previous.pega.com/data-management-and-integration/86/adding-s3-repository)
2. Configure Prediction Studio to use this repository for storage (in Settings)
3. Configure Prediction Studio to use an open ruleset to store the artefacts that will be generated (dataflows) to do the export
4. Run the export (in Prediction Studio, Actions)

After creating the artefacts, the export process will run a-sync. The table exports will be stored under a `/datamart` folder in your repository.

## Configure the Export

|Dev Studio|Prediction Studio|Prediction Studio|
|---|---|---|
|Create a Repository|Prediction Studio Settings|Configure the Export|
|<img src="/pegasystems/pega-datascientist-tools/blob/master/images/ps_dm_export_ds_repo.png">|<img src="/pegasystems/pega-datascientist-tools/blob/master/images/ps_dm_export_ps_storage.png">|<img src="/pegasystems/pega-datascientist-tools/blob/master/images/ps_dm_export_ps_configexport.png"> <img src="/pegasystems/pega-datascientist-tools/blob/master/images/ps_dm_export_ps_configruleset.png">|

## Run the Export

In Prediction studio, start the export from the Actions menu at the top.

<img src="/pegasystems/pega-datascientist-tools/blob/master/images/ps_dm_export_ps_action.png">

## Find the files in your Repository

In your repository, the files will be stored along with meta information.

<img src="/pegasystems/pega-datascientist-tools/blob/master/images/ps_dm_export_s3_sample.png">

# Manual Dataset export from Dev Studio

All the datamart tables are wrapped by datasets that you can export manually from Dev Studio.

From Pega Dev Studio, locate the dataset "pyModelSnapshots" in the Data-Decision-ADM-ModelSnapshot class. This dataset represents a view on the ADM Datamart table with the model snapshots. Export all data from this dataset by clicking "Export" from the "Actions" menu.

<img src="/pegasystems/pega-datascientist-tools/blob/master/images/pega_export_adm_models.png" width="50%">

For detailed instructions see [Exporting data from a data set](https://docs.pega.com/bundle/platform-88/page/platform/decision-management/data-set-export.html).

In the dialog that follows, press "Export". Depending on the size of your data, this may take a while. Then before dismissing the dialog, export the data from the Pega system by clicking the download link that will be shown when the export process has finished.

<img src="/pegasystems/pega-datascientist-tools/blob/master/images/pega_export_dialog.png" width="50%">

The data will be stored in the download location of your browser in the standard Pega dataset export format: zipped, multi-line JSON. You can unzip and load this manually, but we have some utilities in `pdstools` that make this easier for you.

Repeat the steps for the predictor binning data, which is stored in a separate table. You typically want both tables for analysis. Many of the standard plot functions in `pdstools` will require both to be present.

|Data|Class|Dataset|Table|
|---|---|---|---|
|Model Snapshots|Data-Decision-ADM-ModelSnapshot|pyModelSnapshots|PR_DATA_DM_ADMMART_MDL_FACT|
|Predictor Snapshots|Data-Decision-ADM-PredictorBinningSnapshot|pyADMPredictorSnapshots|PR_DATA_DM_ADMMART_PRED|

See (https://docs.pega.com/decision-management-reference-materials/database-tables-monitoring-models) for more information.

## Selective Export to reduce amount of data

Both tables can grow very large. You typically need only the last snapshot (this is also the default configuration) and only the predictor binning for a selection of the models. For example, you may only be interested in the models for the application you are working on, not in all the adaptive models that were ever created in the system, or perhaps in just a particular Channel.

In order to accomplish this, you create your own dataflows with the desired filtering options.

1. Create a dataflow on **Data-Decision-ADM-ModelSnapshot**
2. Source the dataflow with the **pyModelSnapshots** dataset
3. Insert a Filter shape after the source dataset to filter on the models of interest. If you filter by rule that would be a condition on **.pyConfigurationName**.
4. Create a Cassandra dataset (Decision Data Store) as the destination. The keys the system shows when saving it (model ID, snapshot time, application) are fine.

There is an exercise in Pega Academy that covers similar steps, modifying the Prediction Studio based export - but that is really equivalent as this feature just generates the data flow that you build for yourself here. See [Exporting adaptive model data for external analysis in Pega Academy](https://academy.pega.com/challenge/exporting-adaptive-model-data-external-analysis/v2).

Typical filtering options include:
* Model data for certain channels only (pyChannel)
* For certain model configurations (pyModelConfiguration)
* Last 3 months only (expression on snapshot time)


<img src="/pegasystems/pega-datascientist-tools/blob/master/images/ds_mdl_export_dataflow.png">

|Source|Selected Models|Destination|
|---|---|---|
|<img src="/pegasystems/pega-datascientist-tools/blob/master/images/ds_mdl_export_df_source.png">|<img src="/pegasystems/pega-datascientist-tools/blob/master/images/ds_mdl_export_df_filter.png">|<img src="/pegasystems/pega-datascientist-tools/blob/master/images/ds_mdl_export_df_dest.png">|

Run this dataflow and export the destination dataset.

Similar for the Predictor data. After you have exported the models selectively, you probably only want the predictor data for those models. The steps are similar:

1. Create a dataflow on **Data-Decision-ADM-PredictorBinningSnapshot**
2. Source the predictor flow with the **pyADMPredictorSnapshots** dataset
3. Instead of a filter like in the previous one, first add a Compose shape
4. Go back to your model data flow and make the destination abstract. Then use this in the Compose. The condition is equal **pyModelID**, use a new property to hold the model data (single page property of class **Data-Decision-ADM-ModelSnapshot**).
5. Add a filter to filter out any predictor data that does not match
6. Destination is a custom Cassandra dataset like before

<img src="/pegasystems/pega-datascientist-tools/blob/master/images/ds_pred_export_dataflow.png">

|Source|Include Model Data|Selected Models|Destination|
|---|---|---|---|
|<img src="/pegasystems/pega-datascientist-tools/blob/master/images/ds_pred_export_df_source.png">|<img src="/pegasystems/pega-datascientist-tools/blob/master/images/ds_pred_export_df_compose.png">|<img src="/pegasystems/pega-datascientist-tools/blob/master/images/ds_pred_export_df_filter.png">|<img src="/pegasystems/pega-datascientist-tools/blob/master/images/ds_pred_export_df_dest.png">|

Like before, now run this dataflow and export the destination dataset.

# Manual table export from database

The table with the model snapshots is `PR_DATA_DM_DATAMART_MDL_FACT`. You can export this using your favourite database tool. Optionally leave out Pega internal fields (starting with pz/px) and the (large) raw model data field (pymodeldata). 

<img src="/pegasystems/pega-datascientist-tools/blob/master/images/pega_db_models.png" width="50%">

When exporting as a CSV be careful:
* Include a header with the names
* Make sure the column separator does not interfere with characters in the fields - a comma is not safe, the pipe character | is often a better choice
* If possible use double quotes around symbolic values
* If possible use an unambigous standard format for the date/time fields (pySnapshotTime)

Then read the resulting file into R or Python and go from there. 

## Postprocessing

Reading CSV's with embedded comma's can be challenging. The R ADMDatamart function has options for preprocessing where clean-up actions can be specified. If that does not provide enough flexibility read the data first, then pass it in as data objects into the ADMDatamart class/function.

### Python

```python
dm = ADMDatamart(model_filename = "models.csv", predictor_filename = "preds.csv", path="adm")
```

### R

```r
dm <- ADMDatamart("models.csv", "preds.csv", folder="adm")

# Using pre-processing to change quotes can be done using the "cleanup" hooks
# in this case, the pyname field in the CSV ended up having two double quotes like this:
#    {""pyName"":""RideTheWave"",""pyTreatment"":""T63562""}
# so we use the cleanup hook to first change this to:
#    {"pyName":"RideTheWave","pyTreatment":"T63562"}
dm <- ADMDatamart("models.csv", "preds.csv", folder="adm",
                  cleanupHookModelData = function(mdls) { 
                     mdls[, pyname := gsub(pattern = '""', replacement = '"', pyname, ignore.case = T)]
                     return(mdls) } )

# Another example, where the CSV generator used single quotes where double quotes were expected:
#    {'pyName':'RideTheWave','pyTreatment':'T63562'}
# so we use the cleanup hook to change to double quotes:
#    {"pyName":"RideTheWave","pyTreatment":"T63562"}
dm <- ADMDatamart("models.csv", "preds.csv", folder="adm",
                  cleanupHookModelData = function(mdls) { 
                     mdls[, pyname := gsub(pattern = '""', replacement = '"', pyname, ignore.case = T)]
                     return(mdls) } )
```


# Off-line analysis of the exported data

## Reading the data with PDS Tools

### Python

For Python use the files from the GitHub repository directly. Use the `ADMDatamart` class to read the data. The first argument to the `ADMDatamart` class is the directory for the files, if their names have not been changed (i.e. correspond to the default filenames), it should be read automatically.

```python
from pdstools import ADMDatamart
dm = ADMDatamart("~/Downloads")
```


```python
from pdstools import ADMDatamart
dm = ADMDatamart(model_filename = "Data-Decision-ADM-ModelSnapshot_MyModelSnapshots", 
                 predictor_filename = "Data-Decision-ADM-PredictorBinningSnapshot_MyPredictorSnapshots",
                 path = "~/Downloads")
```

### R

In the `pdstools` library, use the [ADMDatamart](https://pegasystems.github.io/pega-datascientist-tools/R/reference/ADMDatamart.html) function to load the ADM Datamart data. This function reads data, drops Pega-internal fields, standardizes the field names and performs other cleanup activities. In addition to dataset exports it can also read CSV, parquet and many other formats.

There also is a generic method to read any dataset (and which will not perform any of these cleanup activities): [readDSExport](https://pegasystems.github.io/pega-datascientist-tools/R/reference/readDSExport.html).

In both functions you can omit the timestamp of the Pega file and it will always take the latest version of the file in the specified location. This is very convenient when you do multiple exports from Pega, so it always takes the latest export.

```r
library(pdstools)
dm <- ADMDatamart(folder = "~/Downloads")
```

```r
library(pdstools)
dm <- ADMDatamart("Data-Decision-ADM-ModelSnapshot_MyModelSnapshots", 
                  "Data-Decision-ADM-PredictorBinningSnapshot_MyPredictorSnapshots",
                  folder = "~/Downloads")
```

## Visualisation of the Datamart

Now the data is retrieved, it is easy to create plots and perform all sorts of analyses. The library provides several standard plots (see plot* functions in the [help](https://pegasystems.github.io/pega-datascientist-tools/R/reference/index.html)), although it is easy enough to construct your own (see source of the plot functions in the repository).

### Python

```python
dm.plotPerformanceSuccesRateBubbleChart()
```

<img src="/pegasystems/pega-datascientist-tools/blob/master/images/datamartplot1.png" width="50%">

### R

```r
plotPerformanceSuccessRateBubbleChart(dm)
```

