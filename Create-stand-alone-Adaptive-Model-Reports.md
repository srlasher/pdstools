CDH Tools includes an R notebook to produce an off-line viewable, stand-alone model report for your ADM models. These reports are similar to the reports in the product but can be generated and browsed off-line. This can be very useful for auditing and regularity requirements.

The reports add some functionality not currently present in the product, like showing the active bins of the propensity mapping, an overview of predictor performance across models in the form of boxplots, and more. They are parameterized and can easily be applied to any export of the ADM datamart.

The notebook is in R and will require R to be installed, but is just a tool and does not require any R skills to be used.

# Sanity check

Before you run it on your own data, check that all is working by running the example:

1. Install R and R Studio and the cdhtools package as per the ["Getting started" in the main page](https://github.com/pegasystems/cdh-datascientist-tools/wiki#getting-started-with-the-r-library)
2. Either check out ("clone") the [CDH Tools repository](https://github.com/pegasystems/cdh-datascientist-tools) from git, or (if you are not comfortable with git), just [download the Model Report notebook](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/examples/datamart/modelreport.Rmd).
3. Open the notebook "examples/datamart/modelreport.Rmd" in R Studio and "Knit to HTML" (or PDF if you have the required libs installed). When finished (it takes a few minutes), a sample report should open in a browser window.

# Creating stand-alone model reports on your own data

There are three parts to this:

1. From Pega, [export the ADM datamart data](How-to-export-and-use-the-ADM-Datamart).
2. Then, using R (or Python), load this data and subset to the models/channels/groups you're interested in. You can skip this step but creating reports on ALL models in the system is usually not what you want, as there may be 1000's of them.
3. Run the notebook. You could do this interactively, using the option to "Knit with Parameters" from R Studio. But if you plan to create more than a few reports, you probably want to un a batch job to create the reports.

Below we go through these steps in detail. Let's assume we're interested in the **OmniAdaptiveModel** reports for the **CreditCards** group in all **Outbound** channels (this example is based on CDH Sample).

## Export the datamart data from Pega

See [export the ADM datamart data](How-to-export-and-use-the-ADM-Datamart)

The model data is usually not gigantic, but the predictor snapshots table can be sizeable. So if a dataset export is not feasible you have two other options:

**Option 1**: Read the data from the database tables directly, using a DB tool of your choice. See notes in above article for some caveats.

**Option 2**: Use a data flow to do filtering before sending the data to a dataset. Source it with the OOTB datasets mentioned above, then apply filtering as needed and write to a dataset of your own (usually a DDS dataset) that you can then export just like the default, OOTB datasets.


## Create files for the models of interest

* Fire up your R Studio and create a script to load these files, filter out only the models of interest and write back as CSV files

```r
# Example preprocessing to create off-line model reports for
# CDH Sample, for CreditCards offers in Outbound channels

library(cdhtools)
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
| <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/R-studio-modelreport-knit-with-params.png"> | <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/R-studio-modelreport-knit-dialog.png"> |



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

cdhtools="~/Documents/pega/cdh-datascientist-tools"
modeloverviewnotebook="$cdhtools/examples/datamart/healthcheck.Rmd"
modelreportnotebook="$cdhtools/examples/datamart/modelreport.Rmd"

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










