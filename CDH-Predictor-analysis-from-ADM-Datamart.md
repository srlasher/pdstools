# This page is not finished yet

This page will become a detailed example of how to use ADM predictor level datamart data.

Explain the table is at bin level but often we're interested in just the predictor level data, not the individual bins.

Retrieving the data
* Simple DS export using readADMDatamartPredictorExport is a possibility but will be less feasible as the data is typically fairly large
* Exporting a subset of the data might be possible, need to give an example (bake into CDH sample?) of a dataflow that sources from PredictorBinningSnapshot and filters. Perhaps it filters by binindex==1 to get only predictor level data.
* DB export is recommended, see getPredictorsForModelsFromDatamart method (TODO name consistently)
* Manual DB export remains an option


Then again one or more examples of the code to create a plot

For more examples refer to R and Python notebooks (for R cdhtools these will probably be vignettes as well)

