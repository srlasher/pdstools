# Welcome to the cdh-datascientist-tools wiki!

These open sourced accompanying tools to Pega CDH (Customer Decision Hub) provide data scientists some tooling to analyze the data produced by a running Pega Decisioning system. It includes tools to easily read Dataset exports, convert Adaptive Models to scorecards, off-line browsable reports on the adaptive models and more. 

Tooling is both in R and Python although currently not everything is available in both languages.

# CDH Graph Gallery

Examples of useful graphs to get insights into CDH using data from the Pega platform.

[CDH Graph Gallery](CDH-Graph-Gallery)

# R Reference

[ADM Datamart Examples](https://pegasystems.github.io/cdh-datascientist-tools/articles/adm-datamart.html)

[Working with Dataset exports](https://pegasystems.github.io/cdh-datascientist-tools/articles/adhoc-datasetanalysis.html)

[Function Reference](https://pegasystems.github.io/cdh-datascientist-tools/reference/index.html)

Notebooks for Model and Predictor reports in the "extra" folder that extend product capabilities.

# Python

Few utils plus two classes for reporting.

# Java utils

There is an experimental tool to work with internal Cassandra.

To build it, run `gradle allinone` in java/DDSUtil. To run, `java -jar java/DDSUtil/build/libs/DDSUtil-1.0-SNAPSHOT.jar`. The tool provides help.
