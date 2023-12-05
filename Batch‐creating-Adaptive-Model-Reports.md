There are various options to create an ADM Health Check or individual Model Report as detailed here.

When you want to create many model reports, and frequently want to update them because new data is available, it can be convenient to run a batch script instead.

The reports are just Quarto Notebooks (for the Python versions), or R Markdown (for the R versions), which take arguments and can be called from a script. The script can be in any scripting language, bash, zsh, python, or R etc. The advantage of using Python/R is that you can easily add a few lines of code to subset the data to the aspects you're interested in (e.g. certain configurations, certain issues) etc.

# Prerequisites

|For the Python versions|For the R versions|
|---|---|
|Python|R|
|[Quarto](https://quarto.org) and [Pandoc](https://pandoc.org)|R Markdown and Pandoc but these come installed with [R Studio](https://posit.co/products/open-source/rstudio/). For our purposes the free version of R Studio is sufficient.|
|[PDSTools python library](https://github.com/pegasystems/pega-datascientist-tools#getting-started)|PDS Tools library|

In addition, you'll need a clone of the [Pega Data Scientist Tools repository](https://github.com/pegasystems/pega-datascientist-tools) from git, as this contains the notebooks and example scripts. You could of course also download just the files/scripts that you plan to use, but cloning the whole repository is easier and makes it easier to stay current with updates.

# Example scripts

We have provided a few sample scripts to get you going. The scripts are rather self-explanatory.

The sample scripts can be found in the examples/datamart folder of the cloned repository.

|Script|Purpose|
|---|---|
|[create_single_model_report_python.sh](https://github.com/pegasystems/pega-datascientist-tools/blob/master/examples/datamart/create_single_model_report_python.sh)|Shell script to create stand-alone Quarto/Python model reports|
|[create_single_model_report_R.sh](https://github.com/pegasystems/pega-datascientist-tools/blob/master/examples/datamart/create_single_model_report_R.sh)|Shell script to create stand-alone Markdown/R model reports|



### TODO below is WIP


## Create files for the models of interest

* Fire up your R Studio and create a script to load these files, filter out only the models of interest and write back as CSV files

```r
# Example preprocessing to create off-line model reports for
# CDH Sample, for CreditCards offers in Outbound channels

library(pdstools)
library(data.table)

# Read the latest ADM export files from the Downloads folder
# and filter to only the models we're interested in.
dm <- ADMDatamart("~/Downloads", filterModelData = function(mdls) { 
   return(mdls[ConfigurationName == "OmniAdaptiveModel" & Group == "CreditCards" & Direction == "Outbound"]) 
} )

# Write back a CSV with only the model data of interest
write.csv(dm$modeldata, "models.csv", row.names = F)

# Write back a CSV with only the predictor data for the models of interest
write.csv(dm$predictordata, "predictors.csv", row.names = F)
```


## Create the HTML or PDF model reports

It is possible to create the reports interactively. Select "Knit with parameters" and fill in the paths to the ADM datamart download and the ID of the model you want to report on. The ID can be found from the Prediction Studio UI (in recent versions), or by loading the model data in R or Python and inspecting it there. The title and description are used in the generated file for informational purposes only.

| Knit option in R Studio | Knit dialog for the stand alone Adaptive Model Report notebook |
| :---: | :---: |
| <img src="/pegasystems/pega-datascientist-tools/blob/master/images/R-studio-modelreport-knit-with-params.png"> | <img src="/pegasystems/pega-datascientist-tools/blob/master/images/R-studio-modelreport-knit-dialog.png"> |



More common is to run this in a batch.

* Using an editor of your choice, create a shell script that invokes R with the appropriate notebooks. First, we run a model overview report that also spits out a list of model ID's. Then we loop over these model ID's and create a model report for each of them.

```bash
#!/bin/bash

# Location of the input data

modeldata="`pwd`/models.csv"
predictordata="`pwd`/predictors.csv"

# Prefix of the generated reports

nameprefix="CDHSample"

# Location of the notebooks

pdstools="~/Documents/pega/pega-datascientist-tools"
modeloverviewnotebook="$pdstools/examples/datamart/healthcheck.Rmd"
modelreportnotebook="$pdstools/examples/datamart/modelreport.Rmd"

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

This will now generate a bunch of reports, each as a single, stand-alone HTML file. As you may have noticed, some portions of the report are interactive and support interactive zooming, selection etc.

The reports by default only include details of the active predictors. This can be changed easily. When making changes to the notebooks, easiest is to run them first from R Studio directly, changing the default parameter values at the top of the script where the source data is specified, then run them in batch if everything is as you want it to be.

If you have modifications or additions to the script that could be useful to others - please share! We're interested in feedback and pull requests!










