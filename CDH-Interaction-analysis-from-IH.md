# This page is not finished yet

This page will become a detailed example of how to use IH data

Explain IH data model high level (star schema)

Retrieving the data
* Simple DS export using base readDSExport (TODO perhaps have specialized version) won't work except for demo scenarios as IH data is large
* Exporting a subset of the data might be possible, with a lot of filtering, need to give an example (bake into CDH sample?) of a dataflow that sources from pxInteractionHistory and filters.
* DB export is recommended, currently only a manual export is supported - TODO build some utils that do the join


Then again one or more examples of the code to create a plot

For more examples refer to R and Python notebooks (for R cdhtools these will probably be vignettes as well)

