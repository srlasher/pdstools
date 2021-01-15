The tools include R notebooks to produce off-line viewable, stand-alone model reports and a model overview. These reports are similar to the reports in the product but can be generated and browsed off-line. They also add some functionality not currently present in the product, like showing the active bins of the propensity mapping, an overview of predictor performance across models in the form of boxplots, and more. They are parameterized and can easily be applied to any export of the ADM datamart.

You will need R installed to use them, but don't need R skills.

# Overview

Before you run it on your own data, check that all is working by running the example:

1. Install R and R Studio as per the "Getting started" in the main page
2. Check out the CDH Tools repository from git
3. Open the example notebook "" in R Studio and "Knit to HTML" (or PDF if you have the required libs installed)

# Creating an off-line Model Report from your own data

There are three parts to this:

1. From Pega, export the ADM datamart data
2. Then, using R or Python, load this data and subset to the models/channels/groups you're interested in. You can skip this step but exporting ALL models in the system is usually not what you want, as there may be 1000's of them.
3. Then finally, run a (batch) job to create the reports. This would typically be from a shell script, although you can of course do both this and the previous step from a single notebook if the proper kernels are in place.

Below we go through these steps in detail. Let's assume we're interested in the **OmniAdaptiveModel** reports for the **HomeLoans** group in all **Outbound** channels (this example is based on CDH Sample).

## Export the datamart data from Pega

1a. From Dev Studio, export the dataset **pyModelData** with applies-to **Data-Decision-ADM-ModelSnapshot**
1b. From Dev Studio, export the dataset **pyADMPredictorSnapshots** with applies-to **Data-Decision-ADM-PredictorBinningSnapshot**

## Create CSV files for the models of interest

2. Fire up your R Studio and create a script to load these files, filter out only the models of interest and write back as CSV files

## Create the HTML or PDF model reports

3. Using an editor of your choice, create a shell script that invokes R with the appropriate notebooks. First, we run a model overview report that also spits out a list of model ID's. Then we loop over these model ID's and create a model report for each of them.







