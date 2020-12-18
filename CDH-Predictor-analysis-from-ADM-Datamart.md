# This page is not finished yet

This page will become a detailed example of how to use ADM predictor level datamart data.

| Predictor performance by Type | TODO | TODO |
| :---: | :---: | :---: |
| [<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/aggregate_predictor_performance.png" width="100%">](/pegasystems/cdh-datascientist-tools/blob/master/images/aggregate_predictor_performance.png) |   |   |

Explain the table is at bin level but often we're interested in just the predictor level data, not the individual bins.

Retrieving the data
* Simple DS export using readADMDatamartPredictorExport is a possibility but will be less feasible as the data is typically fairly large
* Exporting a subset of the data might be possible, need to give an example (bake into CDH sample?) of a dataflow that sources from PredictorBinningSnapshot and filters. Perhaps it filters by binindex==1 to get only predictor level data.
* DB export is recommended, see getPredictorsForModelsFromDatamart method (TODO name consistently)
* Manual DB export remains an option


Then again one or more examples of the code to create a plot

For more examples refer to R and Python notebooks (for R cdhtools these will probably be vignettes as well)

# Bringing it all together

All the predictor analysis plots shown in the [gallery](CDH-Graph-Gallery) can be created using the sample codes from the provided notebooks.

Many of the plots can be re-created using the data provided with **cdhtools**. Using the R package this data is available when the `cdhtools` library is loaded. The raw data files (dataset exports and .csv files) are also available in the /extra folder of the repository. Some plots required such amounts of data that we did not want to include it in the repository. You can still run the code examples and get similar looking plots.

## Python

See the example notebook https://github.com/pegasystems/cdh-datascientist-tools/blob/master/python/Example_ADM_Analysis.ipynb

## R

See the example notebook https://github.com/pegasystems/cdh-datascientist-tools/blob/master/r/vignettes/adm-datamart.Rmd

or when you have `cdhtools` installed, check the vignette `adm-datamart`: 

```r
library(cdhtools)

vignette("adm-datamart")
```


