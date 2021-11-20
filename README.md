# Credit Card Fraud Detection

Introduction Goes Here

### The Credit Card Fraud Dataset

The [Credit Card Fraud Detection dataset](https://www.kaggle.com/mlg-ulb/creditcardfraud) from Kaggle contains 284,807 credit card transactions made in September 2013 by credit cardholders.  Of the 284,807 transactions, 492 (0.172%) are fraudulent.  The dataset consists of 31 columns of numerical data.  Three of the columns are time (number of seconds between the first transaction in the dataset and the current transaction), amount of transaction, and class (1 = fraudulent; 0 = not fraudulent).  The remaining columns are designated as V1, V2, etc.  The variables in these columns were obtained using Principal Component Analysis (PCA) to transform data about each transaction.  We do not know what the original data features are because that information is confidential.

### Creating the Best Machine Learning Model

We used Amazon SageMaker Autopilot to select and build the best machine learning (ML) models for the credit card fraud dataset.  Autopilot, as the name suggests, automates the data preprocessing and model selectiong, building, and tuning.  After we created an Amazon S3 bucket for the ZIP file containing the credit card fraud data CSV file, we split the data into training and testing sets and uploaded them to the Amazon S3 bucket.  Then we configured and launched an Autopilot job to process the data, create a valdiation set, determine which ML  models would work best for our dataset, and tune the top models to determine the optimal hyperparameters.  We configured Autopilot to return the top five best-performing ML models. 

After the Autopilot job completed, we used Amazon SageMaker to deploy an inference pipeline to deploy the top candidate model returned by Autopilot.  We then used batch transform to remove noise and other interferance that might affect model performance and generate predictions using the ML model.

Autopilot correctly determined that a binary classification model was needed for our dataset and selected the best ML model based on F1 score: 0.41008999943733215.  THe accurace of the model is 0.9959099888801575 and area under the curve is 0.9928200244903564.


### Resources
Direct Marketing with Amazon SageMaker AutoPilot: https://github.com/aws/amazon-sagemaker-examples/blob/master/autopilot/sagemaker_autopilot_direct_marketing.ipynb
Training Models to Detect Credit Card Frauds with Amazon SageMaker Autopilot:  https://towardsdatascience.com/training-models-to-detect-credit-card-frauds-with-amazon-sagemaker-autopilot-d49a6b667b2e
AWS Deploy and Inference Pipeline: https://docs.aws.amazon.com/sagemaker/latest/dg/inference-pipelines.html
AWS Use Batch Transform: https://docs.aws.amazon.com/sagemaker/latest/dg/batch-transform.html
