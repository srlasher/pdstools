# This page is not finished yet

This page will become a detailed example of how to use IH data

| Hierarchical breakdown of response counts | Weekly accept rates with outliers | Delta in accept rates |
| :---: | :---: | :---: |
| [<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/IH_responses_hierarchical_breakdown.png" width="100%">](/pegasystems/cdh-datascientist-tools/blob/master/images/IH_responses_hierarchical_breakdown.png) | [<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/IH_weekly_accept_rate_with_outliers.png" width="100%">](/pegasystems/cdh-datascientist-tools/blob/master/images/IH_weekly_accept_rate_with_outliers.png) | [<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/IH_share_delta.png" width="100%">](/pegasystems/cdh-datascientist-tools/blob/master/images/IH_share_delta.png) |

Explain IH data model high level (star schema)

Retrieving the data
* Simple DS export using base readDSExport (TODO perhaps have specialized version) won't work except for demo scenarios as IH data is large
* Exporting a subset of the data might be possible, with a lot of filtering, need to give an example (bake into CDH sample?) of a dataflow that sources from pxInteractionHistory and filters.
* DB export is recommended, currently only a manual export is supported - TODO build some utils that do the join


Then again one or more examples of the code to create a plot

For more examples refer to R and Python notebooks (for R cdhtools these will probably be vignettes as well)

# Bringing it all together

All the response analysis plots shown in the [gallery](CDH-Graph-Gallery) can be created using the sample codes from the provided notebooks.

Many of the plots can be re-created using the data provided with **cdhtools**. Using the R package this data is available when the `cdhtools` library is loaded. The raw data files (dataset exports and .csv files) are also available in the /extra folder of the repository. Some plots required such amounts of data that we did not want to include it in the repository. You can still run the code examples and get similar looking plots.

## Python

See the example notebook [Example_IH_Analysis.ipynb](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/python/Example_IH_Analysis.ipynb)

## R

See the example notebook [ih-reporting.Rmd](https://pegasystems.github.io/cdh-datascientist-tools/articles/ih-reporting.html)

or when you have `cdhtools` installed, check the vignette `ih-reporting`: 

```r
library(cdhtools)

vignette("ih-reporting")
```



