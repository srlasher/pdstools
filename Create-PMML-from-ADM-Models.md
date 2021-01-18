We have experimental support to create PMML Scorecards from ADM models.

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


