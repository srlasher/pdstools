The R package contains experimental support to create PMML Scorecards from ADM models. ADM models can be "frozen" into PMML Scorecards and when executed, these produce not only the same propensities, but also so called "reason codes" that explain the individual decisions.

Every ADM rule (configuration) translates into one PMML file. So all individual models for every context key (every issue/group/action etc) are combined into a single PMML Scorecard model. The results from running the PMML Scorecard are identical to running the ADM models.

Use cases:
* Scorecards can be explained more easily
* ADM Models can be frozen into PMML
* The ADM Models can be executed and results analyzed outside of Pega

The PMML Scorecard can be generated from the ADM datamart or from the internal "Factory" tables. Using the ADM datamart is easiest but does not guarantee 100% identical results because the labels for symbolic predictors will be truncated if there are many. Using the internal tables does not suffer from that problem but is a little more difficult to set up.

These are the steps

1. Export the ADM datamart tables
2. Call the `adm2pmml` work-horse function from `cdhtools`

Then optionally pick up the PMML files and put them into a Pega model or prediction.

# Detailed Scenario

In this example (based on CDH Sample) we will create a PMML Scorecard for the "OmniAdaptiveModel" rule.

```r
library(cdhtools)
library(data.table)
library(XML)

# Read the latest ADM Model export file from the Downloads folder
modelz <- readADMDatamartModelExport("~/Downloads")

# Subset to only the models of interest. Note that the field names have
# been stripped from the "py"/"px" prefixes.
modelz <- modelz[ ConfigurationName == "OmniAdaptiveModel" & Group == "CreditCards" & Direction == "Outbound"]

# Read the most recent ADM Predictor export file from the Downloads folder
predz <- readADMDatamartPredictorExport("~/Downloads", latestOnly = F, noBinning = F)

# Generate the PMML file
adm2pmml(dmModels = modelz, dmPredictors = predz, tmpDir = "~/tmp", verbose = T)
```

Then in Pega, in Prediction Studio you can create a new Predictive Model using this PMML file:

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/pmml_model_import.png" width="50%">




