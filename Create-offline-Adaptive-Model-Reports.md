The tools include R notebooks to produce off-line viewable, stand-alone model reports and a model overview. These reports are similar to the reports in the product but can be generated and browsed off-line. They also add some functionality not currently present in the product, like showing the active bins of the propensity mapping, an overview of predictor performance across models in the form of boxplots, and more. They are parameterized and can easily be applied to any export of the ADM datamart.

You will need R installed to use them, but don't need R skills.

# Overview

Before you run it on your own data, check that all is working by running the example:

1. Install R and R Studio as per the "Getting started" in the main page
2. Check out the CDH Tools repository from git
3. Open the example notebook "extra/modelreport.Rmd" in R Studio and "Knit to HTML" (or PDF if you have the required libs installed). When finished (it takes a few minutes), a sample model report should open in a browser window.

# Creating an off-line Model Report for your own data

There are three parts to this:

1. From Pega, export the ADM datamart data
2. Then, using R or Python, load this data and subset to the models/channels/groups you're interested in. You can skip this step but exporting ALL models in the system is usually not what you want, as there may be 1000's of them.
3. Then finally, run a (batch) job to create the reports. This would typically be from a shell script, although you can of course do both this and the previous step from a single notebook if the proper kernels are in place.

Below we go through these steps in detail. Let's assume we're interested in the **OmniAdaptiveModel** reports for the **CreditCards** group in all **Outbound** channels (this example is based on CDH Sample).

## Export the datamart data from Pega

* From Dev Studio, export and download the dataset **pyModelData** with applies-to **Data-Decision-ADM-ModelSnapshot**
* From Dev Studio, export and download the dataset **pyADMPredictorSnapshots** with applies-to **Data-Decision-ADM-PredictorBinningSnapshot**

The model data is usually not gigantic, but the predictor snapshots table can be sizeable. So if a simple dataset export is not feasible you have two other options:

Option 1: Read the data from the database tables directly, using a db tool of your choice. You may be able to do some filtering there so you only take the data that you need. When exporting directly to a CSV this way, be aware of possible issues with comma's or semicolons as they may interfere with such characters in the names of the actions (propositions). Best is to use a CSV format that also quotes the elements. Be also aware of date formatting. The notebook will apply some commonly used date formats.

Option 2: Use a data flow to do the filtering. Source it with the OOTB datasets mentioned above, then apply filtering as needed and write to a dataset of your own (usually a DDS dataset) that you can then export just like the default, OOTB datasets.


## Create CSV files for the models of interest

* Fire up your R Studio and create a script to load these files, filter out only the models of interest and write back as CSV files

```r
# Example preprocessing to create off-line model reports for
# CDH Sample, for CreditCards offers in Outbound channels

library(cdhtools)
library(data.table)

# Read the latest ADM Model export file from the Downloads folder
modelz <- readADMDatamartModelExport("~/Downloads")

# Subset to only the models of interest. Note that the field names have
# been stripped from the "py"/"px" prefixes.
modelz <- modelz[ ConfigurationName == "OmniAdaptiveModel" & Group == "CreditCards" & Direction == "Outbound"]

# Read the latest ADM Predictor export file from the Downloads folder
predz <- readADMDatamartPredictorExport("~/Downloads")

# Write back a CSV with only the model data of interest
write.csv(modelz, "models.csv", row.names = F)

# Write back a CSV with only the predictor data for the models of interest
write.csv(predz[ModelID %in% modelz$ModelID], "predictors.csv", row.names = F)
```


## Create the HTML or PDF model reports

It is possible to create the reports interactively. Select "Knit with parameters" and fill in the required info. But more common is to run this in a batch.

* Using an editor of your choice, create a shell script that invokes R with the appropriate notebooks. First, we run a model overview report that also spits out a list of model ID's. Then we loop over these model ID's and create a model report for each of them.

```bash
#!/bin/bash

# Location of the GIT checkout of the CDH tools

cdhtools="~/Documents/pega/cdh-datascientist-tools"

# Location of the input data

modeldata="`pwd`/models.csv"
predictordata="`pwd`/predictors.csv"

# Prefix of the generated reports

nameprefix="CDHSample"

# Location of the notebooks (unless copied/modified these defaults should be fine)

modeloverviewnotebook="$cdhtools/extra/adaptivemodeloverview.Rmd"
modelreportnotebook="$cdhtools/extra/modelreport.Rmd"

# Generated file with the model ID's. You don't provide this file, this is generated
# by the model overview notebook.

modellist="cdhsample-modellist.txt"

# Run the model overview notebook

R -e "rmarkdown::render('$modeloverviewnotebook',params = list(modelfile='$modeldata', modellist='`pwd`/$modellist', predictordatafile='$predictordata'), output_file='`pwd`/$nameprefix\ Model Overview with Predictor Overview.html')"
```

After this first step, there should be an output file "CDHSample Model Overview.html". Open that in a browser, it should give a similar view as the bubble charts in the product. 

In addition, there should be a file "cdhsample-modellist.txt" with the list of model ID's that we selected.

The second half of the script iterates over this list and creates an HTML file for each of these models.

```bash
while read p; do
modelID="$(cut -d';' -f1 <<<"$p")"
modelName="$(cut -d';' -f2 <<<"$p")"

R -e "rmarkdown::render('$modelreportnotebook',params = list(predictordatafile='$predictordata', modeldescription='$modelName', modelid='$modelID'), output_file='`pwd`/$nameprefix Model Report $modelName.html')"

done <$modellist
```









