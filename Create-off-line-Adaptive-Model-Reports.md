The tools include R notebooks to produce off-line viewable, stand-alone model reports and a model overview. These reports are similar to the reports in the product but can be generated and browsed off-line. They also add some functionality not currently present in the product, like showing the active bins of the propensity mapping, an overview of predictor performance across models in the form of boxplots, and more. They are parameterized and can easily be applied to any export of the ADM datamart.

You will need R installed to use them, but don't need R skills.

Running the Example

1. Install R and R Studio as per the "Getting started" in the main page
2. Check out the CDH Tools repository from git
3. Open the example notebook "" in R Studio and "Knit to HTML" (or PDF if you have the required libs installed)

Creating an off-line Model Report from your own data

1. Export the ADM datamart data
2. Optionally (but typically necessary) identify the models you are interested in and subset to only that data
3. Run a (batch) job to create the reports

