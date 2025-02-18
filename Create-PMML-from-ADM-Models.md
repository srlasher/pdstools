The R package contains experimental support to create PMML Scorecards from ADM models. ADM models can be "frozen" into PMML Scorecards and when executed, these produce not only the same propensities, but also so called "reason codes" that explain the individual decisions.

Every ADM rule (configuration) translates into one PMML file. So all individual models for every context key (every issue/group/action etc) are combined into a single PMML Scorecard model. The results from running the PMML Scorecard are identical to running the ADM models.

Use cases:

* Scorecards can be explained more easily
* Migration of the model learnings between incompatible frameworks - e.g. from a custom framework with a different class structure into NBA-Designer: the ADM models from the old framework can be "frozen" into PMML, then the PMML models can be executed in the new framework while the new ADM models are learning
* The ADM Models can be executed and results analyzed outside of Pega

These are the steps

1. From Pega, export the ADM datamart tables (explained [here](How-to-export-and-use-the-ADM-Datamart) in detail)
2. From an R script, call the `adm2pmml` function from `pdstools`

Then pick up the generated PMML files and put them into a Pega model or prediction.

To run this on the sample data shipped with Pega Data Scientist Tools:

```r
library(pdstools)
library(data.table)
library(XML)
data(adm_datamart)

adm2pmml(adm_datamart, verbose=T)
```

# Detailed Scenario

In this example (based on CDH Sample) we will create a PMML Scorecard for the "OmniAdaptiveModel" rule.

```r
library(pdstools)
library(data.table)
library(XML)

# Read the exported ADM datamart data from the Downloads folder, and
# select one of the models. Make sure to only take the latest snapshot
# of the predictor data.
dm <- ADMDatamart("~/Downloads", 
                  filterModelData = function(mdls){
                     mdls[ConfigurationName == "OmniAdaptiveModel" & 
                          Group == "CreditCards" & 
                          Direction == "Outbound"]})

# Generate the PMML file(s)
adm2pmml(dm)
```

This will now create a file "OmniAdaptiveModel.pmml" in the current folder. 

If you open the generated file you should find a "TreeModel" for each of the context key combinations. The ID of the ADM model can be found back in the `id` element.

<img src="/pegasystems/pega-datascientist-tools/blob/master/images/generated_pmml_snippet.png" width="100%">

Then in Pega, in Prediction Studio you can create a new Predictive Model using this PMML file:

<img src="/pegasystems/pega-datascientist-tools/blob/master/images/pmml_model_import.png" width="50%">

Pega Prediction Studio will see this PMML as an "ensemble" model because it does not contain just a single scorecard, but a lot of them, one for every context key combination.

The inputs will need to be mapped:

1. For context keys (.pyName, .pyGroup etc.) you need to create a Parameter and reference 
2. Regular customer fields should be mapped to the corresponding fields in the Pega class
3. IH predictors will need to be mapped to parameters and provided at runtime via an IH summary shape






