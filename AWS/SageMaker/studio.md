# Amazon SageMaker Studio

### Pipelines

- Where you can build, automate and manage workflows for entire machine learning life cycle.

### Deployments

- Where you can deploy models via an API endpoint.

# Preparing Dataset

## Analyzing and preparing data

1. collect
1. clean and preprocess
1. prepare

### Collect

- collect data from multiple sources

### Clean and preprocess

- manage missing data
  - fill missing values with
    - mean
    - median
    - mode
    - or drop the records entirely
- remove duplicates
- correct data types
- standardize formats

### Preprocess data

- Normalize/Scaling
  - mean normalization
- Feature Engineering
  - create new features
- Encoding categorical variables
  - one hot encoding

## Using data preparation tools - Data Wrangler

Automates repetitive steps to speed data-preparation
Open Data Wrangle by clicking

> Data -> Data Wrangler -> Open in Canvas

**Features of Data Wrangler**:

- Data import
  - You can easily import data from various sources, such as Amazon S3, Athena, Redshift, or even local files.
    - Click on import and prepare -> Tabular -> Select a data source (Option S3) -> S3 bucket name -> select file name
- Data Visualization
  - allows to visualize data using hist, bar, scatter plot and others to better understand data
    - Click on Analysis tab
      - Analysis type
      - Analysis Name
      - Target column
      - problem type (Classification/regression)
      - data size ( sampled dataset or full dataset)
- Built-in transformations
  - Data Wrangler comes with a wide variety of pre-built transformations, from scaling and normalization to encoding categorical variables, so we don't have to write code for each step.
    - Add the transformations by clicking the `+` button -> Add transform
    - Few transformations available
      - Encode categorical
      - Featurize date/time
      - Manage Columns
      - Process numeric (Feature scaling)
    - We can use preview before applying the transformation.
- Custom transformation
  - If required more control, we can write custom scripts directly withing Data Wrangler
- Automated Workflows
  - It can create automated data processing workflows which can be exported to SageMaker notebooks for further analysis or integrating into machine learning pipeline.
  - We can export the transformation by using Export feature of Data Wrangles
    - Export transformation by clicking `+` button -> Export
      - Export data to Canvas dataset
      - Export data to Amazon S3
      - Export via Jupyter notebook
        - Download a local copy
          - A jupyter notebook
          - A data flow file will be downloaded
          - We can upload these 2 files in JupyterLab in Studio then run all the cells.
          - This notebook is loading our data to SageMaker Feature Store, and then at a future time we'll pull our data from Feature Store in order to train our model
        - Export to S3 bucket
      - Add a destination

# Training a Model

## Learning the steps to train a model

How you model learns to make predictions based on patterns in your dataset

- split the data
- Choose learning Algorithms
- Set Hyperparameter
- Select evaluation metrics
- configure training environment
- Train model

## Choosing an algorithm

SageMaker has 15 built in learning algorithms that are optimized for various tasks like classification, regression and forecasting.

- Common Algorithms
  - Linear Learner
  - XGBoost
  - KMeans
    - Useful for Clustering
  - Random Cut Forest(RCF)
    - Detects anomalies in time-series data
  - Object2Vec Algorithm
  - PCA
  - latent Dirichlet Allocation
  - Neural Topic model
  - Image Classification MXNet
  - K Nearest Neighbors

Choose one depends on
|Linear Learner | XGBoost|
|-|-|
Simple relationships | Complex patterns
Small data sets| Large datasets
Speed|Computation resources
Interpritability|More Time|

## Training a model

### Prerequisites

There are few prerequisites to train a model in SageMaker

- Feature store
  - We have created it in DataWrangler
  - We have to execute all the cells in Notebook and find `Feature Group Name` from the results.
- If Access denied error for repository then look for 2 variables in FeatureStore Notebook and make sure both are correct
  - container_uri
  - container_uri_pinner

SageMaker Automatically provide the necessary computation to train the model.

We can check the model progress in the `Jobs` section in SageMakerStudio.
Click on the Job to see more details about the job.

- Performance
- artifacts
- final model artifact location
- security
- hyperparameter's
- configurations
- instances
- logs
- tags
