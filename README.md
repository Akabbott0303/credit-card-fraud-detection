# Credit Card Fraud Detection

Credit card fraud is a modern problem that affects everyone to some degree.  It costs card-issuing banks time, money, and human resources to track and resolve, and it costs consumers time and also money if fraudulent transactions are not caught.  It also shakes our sense of security.  Technology has evolved to create many machine learning models that can be used to detect credit card fraud.  Machine learning models can be built using code and data science libaries, and they can also be built using tools such as Amazon Web Services Autopilot program.  

Which approach to building machine learning models to detect credit card fraud is best - a coding or non-coding approach?  Also, which type of model from the myriad choices available works the best?  These are the questions we set out to answer using a done-for-us dataset and several data science tools.

### The Credit Card Fraud Dataset

For our data, we used the famous [Credit Card Fraud Detection dataset](https://www.kaggle.com/mlg-ulb/creditcardfraud) from Kaggle.  This dataset contains 284,807 credit card transactions made in September 2013 by credit cardholders.  Of the 284,807 transactions, 492 (0.172%) are fraudulent.  

!(/Images/Target_Imbalance.PNG)

The dataset consists of 31 columns of numerical data.  Three of the columns are time (number of seconds between the first transaction in the dataset and the current transaction), amount of transaction, and class (1 = fraudulent; 0 = not fraudulent).  The remaining columns are designated as V1, V2, etc.  The variables in these columns were obtained using Principal Component Analysis (PCA) to transform data about each transaction.  We do not know what the original data features are because that information is confidential but we can see that several of the top 10 most predictive features are correlated with each other.

!(/Images/Correlation_Matrix.PNG)

### Creating the Best Machine Learning Model

We set out to discover which machine learning model might be the best for predicting credit card fraud in the real world.  We used several tools in the data science toolbox to find our best model.

#### Amazon SageMaker Autopilot

Our first step was to use Amazon SageMaker Autopilot to select and build two machine learning (ML) models for the credit card fraud dataset.  Autopilot, as the name suggests, automates the data preprocessing and model selectiong, building, and tuning.  We modeled our code on that in the GitHub repository [Direct Marketing with Amazon SageMaker AutoPilot](https://github.com/aws/amazon-sagemaker-examples/blob/master/autopilot/sagemaker_autopilot_direct_marketing.ipynb).  

We ran two instances of Autopilot.  For the first, we accepted the default inputs and let Autopilot select the best type of model and metric for our data.  Autopilot correctly selected binary classification for the model type and selected F1 score for the metric used to measure which model would be best.  

For imbalanced datasets like the credit card fraude dataset, the area under the Receiver Operating Characteristic (ROC) curve (AUC) is a better metric to use than F1 score. The AUC measures the true positive rate versus the false posictive rate to judege model performance.  For our second Autopilot instance, we used AUC as the defining metric.  

For both instances of Autopilot, the program used RobustScaler to scale the data.  After building and running the models, it selected an XGBoost model as the best in both cases. (The details of the data processing and parameters of the model can be seen in the Data Exploration and Candidate Exploration notebooks.  These notebooks were generated by Autopilot.)  After we had our Autopilot models, our next step was to build our own models, including our own XGBoost model. 

#### Home Grown Models

To test the efficacy of Amazon Autopilot, we built five of our own models using the Scikit-Learn library:

1. Decision Tree
2. Gradient Boosting
3. Random Forest
4. Bagging Classifier
5. XGBoost

The Decision Tree is a supervised learning method.  In classification problems, it predicts target values by using the features of a dataset to make decisions.

The Gradient Boosting Classifier is an additive model that combines other models together to create one model that performs better than its parts.

The Random Forest Classifier is a meta estimator that creates several decision trees from sub-sets of data and averages the results of each to make predictions.

The Bagging Classifier is an ensemble meta estimator.  Like Random Forest, it uses sub-sets of the data.  The predictions from fitting classifiers on the data subsets are aggregated or averaged to generate model predictions.

XGBoost appears to be the current "it" model for classification problems.  This is the model AWS Autopilot selected as the best for the credit card fraud dataset.  XGBoost is "an implementation of gradient boosted decision trees designed for speed and performance" (Brownlee, *XGBoost*).

### Model Evaluation

As noted above, the area under the Receiver Operating Characteristic (ROC) curve (AUC) is the best metric to use for imbalanced datasets. The AUC measures the true positive rate versus the false posictive rate.  A higher AUC score indicates that a particular model performs its classification task well.

AWS Autopilot calculated the AUC score as well as the F1 score and accuracy for its model as part of the code we used.

We used Scikit-Learn to return and plot the ROC for all of our models.  We also printed the AUC, F1 score, accuracy, classification report, and confusion matrix for all models and ploted the precision-recall curve.  Here is a comparison of the metrics our Autopilot notebooks pulled with the same metrics we calculated for our models:

| Model                      | AUC Score   | F1 Score   | Accuracy   |
| -------------------------- | ----------- | ---------- | ---------- |
| AWS AUC Optimzed           | 0.9926      | 0.4532     | 0.9967     |
| AWS F1 Score Optimized     | 0.9688      | 0.4108     | 0.9962     |
| Decision Tree              | 0.8707      | 0.7574     | 0.9992     |
| Gardient Boosting          | 0.8081      | 0.6407     | 0.9988     |
| Random Forest              | 0.9041      | 0.8818     | 0.9996     |
| Bagging Classifier         | 0.8291      | 0.7822     | 0.9994     |
| XGBoost                    | 0.9000      | 0.8688     | 0.9996     |


### Conclusions

The overall top-performing model is the Autopilot-generated XGBoost model optimized for AUC score.  Of the models we built ourselves, the top performer is the Random Forest.  We would have liked to run more metrics for our Autopilot models to compare them more fully to our models, but we found the coding a bit tough to decipher in the time we had for the project.  Amazon has a brand new tool called Canvas that makes building machine learning very easy even for non-programmers, and having worked with Autopilot on the coding end, we can see that there is definitely a need for a tool like this. In addition, Amazon regularly updates Autopilot, adding additional metrics, etc.

Another avenue we would explore if we had additional time is resampling.  We'd like to test the "home grown" models with data resampled using different techniques to see how model performance might be improved.  Of course if time were no object, we'd also like to test more model types.

Detecting credit card fraud is an important endeavor that comes with many complexities.  The data comes with many features, most of which are sensitive in terms of privacy and need to handled with utmost care.  The sheer volume of data adds another layer of complexity.  A tool such as Amazon Autopilot is ideal for a complex tast like detecting credit card fraud.  We are looking forward to continuing our research and seeing what enhancements Amazon makes to the program in the future.

### Resources

Adithyan, Nikihil. *Credit Card Fraud Detection with Machine Learning in Python.* https://medium.com/codex/credit-card-fraud-detection-with-machine-learning-in-python-ac7281991d87. Retrieved 2 December 2021.

Amazon Web Serices. *AWS Deploy and Inference Pipeline*. https://docs.aws.amazon.com/sagemaker/latest/dg/inference-pipelines.html. Retrieved November 21, 2021.

Amazon Web Serices. *AWS Use Batch Transform*. https://docs.aws.amazon.com/sagemaker/latest/dg/batch-transform.html. Retrieved November 21, 2021.

Brownlee, Jason. *A Gentle Introduction to the Gradient Boosting Algorithm for Machine Learning.*. https://machinelearningmastery.com/gentle-introduction-gradient-boosting-algorithm-machine-learning/. Retrieved 3 December 2021.

Brownlee, Jason. *A Gentle Introduction to XGBoost for Applied Machine Learning.*. . Retrieved 3 December 2021.
https://machinelearningmastery.com/gentle-introduction-xgboost-applied-machine-learning/.

Brownlee, Jason. *Bagging and Random Forest for Imbalanced Classification*. https://machinelearningmastery.com/bagging-and-random-forest-for-imbalanced-classification/. Retrieved 2 December 2021.

Cortez, Victoria. *Understanding the ROC curve in three visual steps*. https://towardsdatascience.com/understanding-the-roc-curve-in-three-visual-steps-795b1399481c. Retrieved 3 December 2021.

Karpur, Ajay, et al. *Customer Churn Prediction with Amazon SageMaker Autopilot*. https://github.com/aws/amazon-sagemaker-examples/blob/master/autopilot/autopilot_customer_churn.ipynb. Retrieved November 22, 2021.

Samuel, James. *Training Models to Detect Credit Card Frauds with Amazon SageMaker Autopilot*. https://towardsdatascience.com/training-models-to-detect-credit-card-frauds-with-amazon-sagemaker-autopilot-d49a6b667b2e. Retrieved November 21, 2021.

Severtson, Roald Bradley, et al. *Direct Marketing with Amazon SageMaker AutoPilot*. https://github.com/aws/amazon-sagemaker-examples/blob/master/autopilot/sagemaker_autopilot_direct_marketing.ipynb. Retrieved November 21, 2021.

Steen, Doug.  *Understanding the ROC Curve and AUC*. https://towardsdatascience.com/understanding-the-roc-curve-and-auc-dd4f9a192ecb. Retrieved 3 December 2021.

Sun, Luke.  *Credit Card Fraud Detection*. https://towardsdatascience.com/credit-card-fraud-detection-9bc8db79b956. Retrieved 2 December 2021.
