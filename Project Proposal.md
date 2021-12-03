### Project Proposal

We are going to work with the [Credit Card Fraud Detection dataset](https://www.kaggle.com/mlg-ulb/creditcardfraud) from Kaggle.  This dataset contains 284,807 credit card transactions made in September 2013 by credit cardholders.  Of the 284,807 transactions, 492 (0.172%) are fraudulent.  The dataset consists of 31 columns of numerical data.  Three of the columns are time (number of seconds between the first transaction in the dataset and the current transaction), amount of transaction, and class (1 = fraudulent; 0 = not fraudulent).  The remaining columns are designated as V1, V2, etc.  The variables in these columns were obtained using Principal Component Analysis (PCA) to transform data about each transaction.  We do not know what the original data features are because that information is confidential.  

The questions will be asking of our data are:

1. Which features appear to be indicative of credit card fraud?
2. How are the time and amount features related to predictions of fraud?

Due to the size of the file, we are going to use Amazon Web Services SageMaker to run our code.  After preparing our data using pandas and scikit-learn, we will use the AutoGluon library to produce machine learning models and determine which ones work the best for this dataset.  We'll take the top two models and analyze them to determine which model performs the best.  The dataset is highly imbalanced, containing many more legtitimate transactions than fraudulent transactions, so the recommendation from Kaggle is to use Area Under Precision Recall Curve to measure model accuracy.  Once we have our model, we will explore how it could be used to help banks detect credit card fraud with their own data.