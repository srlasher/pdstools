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
|[create_health_check_python.sh](https://github.com/pegasystems/pega-datascientist-tools/blob/master/examples/datamart/create_health_check_python.sh)|Shell script to create stand-alone Quarto/Python ADM Health Check|
|[create_health_check_R.sh](https://github.com/pegasystems/pega-datascientist-tools/blob/master/examples/datamart/create_health_check_R.sh)|Shell script to create stand-alone Markdown/R ADM Health Check|
|[create_single_model_report_python.sh](https://github.com/pegasystems/pega-datascientist-tools/blob/master/examples/datamart/create_single_model_report_python.sh)|Shell script to create stand-alone Quarto/Python model reports|
|[create_single_model_report_R.sh](https://github.com/pegasystems/pega-datascientist-tools/blob/master/examples/datamart/create_single_model_report_R.sh)|Shell script to create stand-alone Markdown/R model reports|
|[batch_create_reports.R](https://github.com/pegasystems/pega-datascientist-tools/blob/master/examples/datamart/batch_create_reports.R)|Extensive example to create both the overall Health Check as well as individual model reports, generating both the Python and R versions with examples of customization. The script is written in R but it creates both the R and Python versions of the reports. You'll need both sets of prerequisites in order to run this.|



R -e "rmarkdown::render('$modelreportnotebook',params = list(predictordatafile='$predictordata', modeldescription='$modelName', modelid='$modelID'), output_file='`pwd`/$nameprefix Model Report $modelName.html')"

done <$modellist
```

This will now generate a bunch of reports, each as a single, stand-alone HTML file. As you may have noticed, some portions of the report are interactive and support interactive zooming, selection etc.

The reports by default only include details of the active predictors. This can be changed easily. When making changes to the notebooks, easiest is to run them first from R Studio directly, changing the default parameter values at the top of the script where the source data is specified, then run them in batch if everything is as you want it to be.

If you have modifications or additions to the script that could be useful to others - please share! We're interested in feedback and pull requests!










