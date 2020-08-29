![#1589F0](https://placehold.it/15/1589F0/000000?text=+) **Dataset**

This repository contains codes and data for the bank marketing dataset available on [Kaggle](https://www.kaggle.com/volodymyrgavrysh/bank-marketing-campaigns-dataset) and on [this UCI repository](https://archive.ics.uci.edu/ml/datasets/bank+marketing).


![#1589F0](https://placehold.it/15/1589F0/000000?text=+) **Problem overview:**

A bank wants to increase revenue by getting more customers to subscribe to long-term deposits. Based on data from past telemarketing campaigns, we want to predict which clients are more likely to subscribe, so that the bank can target these customers and improve their conversion rate.


![#1589F0](https://placehold.it/15/1589F0/000000?text=+) **Folder structure:**

```bash
│   
├── codes
│   ├── baseline.ipynb (code for baseline performance)
│   ├── modify-v1.ipynb (modification on baseline.ipynb by using cyclic feature engineering)
│   ├── modify-v2.ipynb (modification on modify-v1.ipynb by clustering customers into different groups to create a more concise set of features.)
│    
|
├── data
│   ├── bank-additional-full.csv (original dataset)
│   ├── processed-train-baseline.csv (processed training dataset from baseline.ipynb)
|   ├── processed-test-baseline.csv (processed test dataset from baseline.ipynb)
│   ├── processed-train-modify-v1.csv (processed training dataset from modify-v1.ipynb)
|   ├── processed-test-modify-v1.csv (processed test dataset from modify-v1.ipynb)
│   ├── processed-train-modify-v2.csv (processed training dataset from modify-v2.ipynb)
|   ├── processed-test-modify-v2.csv (processed test dataset from modify-v2.ipynb)

```
If you just want to look at the code with the best results, see modify-v2.ipynb and the corresponding processed train and test datasets. The other codes are only kept to show how different feature engineering techniques helped improve the classifier performance.


![#1589F0](https://placehold.it/15/1589F0/000000?text=+) **Highlights:**

- The dataset is **imbalanced**, with only 11% of the customers called subscribing to long-term deposits. This means a majority classifier would easily get around 90% classification accuracy, but would be pretty useless. So I used **ROC-AUC** (Area Under the ROC curve) instead of classification accuracy as a more effective metric for classifier performance.
- The features are a mix of **numerical** and **categorical** data. The features can be divided into 3 broad categories: **customer data** (age, marital status, job, etc.), **campaign data** (number of calls to the customer during this campaign, number of days since last contact, etc.) and **economic data** (employment variation rate, consumer price index, etc.).


![#1589F0](https://placehold.it/15/1589F0/000000?text=+) **Feature engineering, Models and Results:**

**Baseline code:**. 
 
 - To set up a baseline, I tried the following vanilla algorithms with default parameters: logistic regression, k-nearest neighbors, decision tree, random forest, and XGBoost classifiers. XGBoost and Logistic regression performed best. 
 - With the top two candidates, I used Grid Search to find the best hyperparameters. XGBoost had the best ROC-AUC score.
 - With Recursive Feature Elimination, I selected the top 10 features using Random Forest classifier to see if this subset of features can perform better or as well as the full set of features. XGBoost still had the best performance, with a very slight drop in performance compared to the full feature set. But feature dimensionality was reduced from 33 to 10, considerably reducing training time and model complexity.
 
 **Modification 1:**
 
 - From the feature selection procedure in the above code, 'month' and 'day_of_week' were seen to be among the top 10 features. Because they are ordinal categorical features, I had previously used label-encoding to encode them. However, they are also cyclic in nature (Monday repeats after Sunday and January repeats after December). So taking inspiration from [this blog](http://blog.davidkaleko.com/feature-engineering-cyclical-features.html), I tried using cyclic feature engineering to encode these two variables to see if it improves the performance.
 - There was a marked improvement in the train and test ROC-AUC scores with XGBoost using the full set of features, while the performance using the reduced set remained similar.
 
 **Modification 2:**
 
 - There are broadly 3 types of features in our dataset: **customer data**, **campaign data** and **economic data**. There are 7 features under customer data: 1 numeric (age), and the rest nominal categorical (job, marital status, education, default status, housing loan status and personal loan status). I wanted to see if it was possible to group customers into different clusters and create a new feature for each client depending on which cluster he/she falls into. This might be useful in several ways:  
 1. The number of features would go down, since we just have a cluster number for each customer instead of 7 features belonging to customer data. This would make training faster and our models less complicated.  
 2. It is possible that some of the features for some clients are noisy. Feeding these features directly to our models might hinder them from learning the right signals. Grouping similar customers together using unsupervised learning would smooth out these noisy data points.  
 3. If we know how different customer groups are likely to subscribe to long-term deposits, it would be easier for us to come up with different marketing strategies for each group, or maybe give focus more on high-potential customer groups to improve our conversion rate.
 - Since features pertaining to customer data were of a mixed type (both numerical and categorical), I used k-Prototypes clustering to come up with a new feature. Dropping the original features and introducing the new feature for customer group instead further boosted the ROC-AUC score.
 
 My results, along with some other results for this dataset I could find online, are tabulated below. Please note that most analyses with this dataset uses 'duration' as a feature, thus easily getting an ROC-AUC score > 0.9. However as mentioned previously, since this feature is not known beforehand, we should not use it to build models with real predictive power. As such, I have not included these analyses in the table below.
 
 


![#1589F0](https://placehold.it/15/1589F0/000000?text=+) **Insights into customer trends:**
