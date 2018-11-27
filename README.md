

# Deploying Python models on a Kubernetes Cluster for real-time scoring

## Overview
This scenario shows how to deploy a Frequently Asked Questions (FAQ) matching model as a web service to provide predictions for user questions. For this scenario, “Input Data” in the architecture diagram refers to text strings containing the user questions to match with a list of FAQs. The scenario is designed for the Scikit-Learn machine learning library for Python but can be generalized to any scenario that uses Python models to make real-time predictions.

## Design

The 10K foot view of the architecture that includes things like (DockerHub/VM/etc) to give a visual on what the overall design is.

This should also include supported platforms.

# Prerequisites

What do you need to have at your disposal?

## Databricks cluster
https://ms.portal.azure.com/#create/Microsoft.Databricks

## Databricsk CLI
https://github.com/databricks/databricks-cli

# Setup

Login to databricks CLI. 

## Import Notebooks

`databricks workspace import_dir notebooks /Users/<uname@example.com>/notebooks`

## Setup databricks jobs 

`databricks jobs create --json-file jobs/01_CreateDataIngestion.json`

`databricks jobs run-now --job-id <jobID>`

 
`databricks jobs create --json-file jobs/02_CreateFeatureEngineering.json`

`databricks jobs run-now --job-id <jobID>`

`databricks jobs run-now --job-id <jobID> --notebook-params {"FEATURES_TABLE":"testing_data","Start_Date":"2015-11-15","zEnd_Date":"2017-01-01"}`

On windows command line, we need to escape the double quotes:

`databricks jobs run-now --job-id <jobID> --notebook-params {\"FEATURES_TABLE\":\"testing_data\",\"Start_Date\":\"2015-11-15\",\"zEnd_Date\":\"2017-01-01\"}`


`databricks jobs create --json-file jobs/03_CreateModelBuilding.json`

`databricks jobs run-now --job-id <jobID>`

`databricks jobs run-now --job-id <jobID> --notebook-params {\"model\":\"DecisionTree\"}`


`dbfs cp <SRC> <DST>`

`dbfs cp  -r model.pqt dbfs:/storage/models/model.pqt`

`databricks jobs run-now --job-id <jobID> --notebook-params {\"FEATURES_TABLE\":\"scoring_input\",\"Start_Date\":\"2015-12-30\",\"zEnd_Date\":\"2016-04-30\"}`

`databricks jobs create --json-file jobs/04_CreateModelScoring.json`


# Steps

Instructions on where to go (first notebook or folder)

# Cleaning up

Where applicable, what does the user have to manually scrub to clean it.

# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

### Author: John Ehrlinger <john.ehrlinger@microsoft.com>
