The ADM Health Check provides a generic overview of the ADM models in your system, including charts like the "Bubble Chart" and many more, with recommendations. It can be used as-is or be used as a basis for further, specific analyses. The ADM Model Reports give a detailed view of an individual ADM model, including the binning details of all the predictors which can help drive insights.

Technically, both are Quarto markdown documents using snippets of Python code from the PDS Tools libraries. We also have legacy R versions of both (in R markdown).

There are a number of options to generate the Health Check reports:

|Option|When to use|Instructions|
|---|---|---|
|Github Codespace|Run Health Check in the cloud, directly from GitHub without the need to install any tools|Instructions [below](#github-codespace-option)|
|Standalone Python app|Run a python-based application locally, need to install Python, PDS tools and supporting libraries, but no coding skills required|See [instructions on the Health Check application](https://pegasystems.github.io/pega-datascientist-tools/Python/articles/HealthCheckSetUp.html)|
|VSCode or other IDE|Run the notebooks from a developer environment. Allows for customizations but requires coding skills.|TODO|
|Batch runs|Run the reports in batch - especially convenient when you need to create many individual model reports, or when doing this regularly. Supporting example batch scripts are available, requires some (light) scripting skills and an environment with Python, PDS tools and supporting libraries.|TODO|

# Github Codespace option

## What is it?

Run Health Check in the cloud, directly from GitHub without the need to install any tools. A [Github codespace](https://docs.github.com/en/codespaces) is a development environment that's hosted in the cloud. Each codespace you create is hosted by GitHub in a Docker container, running on a virtual machine.

## When would I use it?

* When you do not or cannot have Python or R installed locally on your computer.
* When you want to quickly run an ADM health check report with no need for local set up.

## What are the considerations?

* This analysis is not processed on your own device, you're running the code on GitHub.

* You will load your ADM snapshot data into the GitHub codespace. This data will be limited to the model and predictor data from ADM.
You can review the structure of the two datasets [here](https://docs-previous.pega.com/decision-management-reference-materials/database-tables-monitoring-models).
 
* You should review your corporate security policies and ensure that using GitHub cloud codespaces is permitted. Reference the [GitHub codespace security policies](https://docs.github.com/en/codespaces/reference/security-in-github-codespaces).

* Pega will not have visibility of any usage statistics/telemetry data/real data you load into this space.

## What are the steps to use it?

1. Navigate to the main [PDS Tools page on GitHub](https://github.com/pegasystems/pega-datascientist-tools).

1. Click on the green Code button and choose to add a codespace

1. Ensure that pop ups are allowed, as the Health Check application will open in a new browser tab

1. Follow the [instructions on the Health Check application](https://pegasystems.github.io/pega-datascientist-tools/Python/articles/HealthCheckSetUp.html#Using-the-App:-A-Step-by-Step-Guide)



