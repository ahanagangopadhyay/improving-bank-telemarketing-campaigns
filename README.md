![#1589F0](https://placehold.it/15/1589F0/000000?text=+) **Dataset**

This repository contains the data and my codes for the bank marketing dataset available on [Kaggle](https://www.kaggle.com/volodymyrgavrysh/bank-marketing-campaigns-dataset) and on [this UCI repository](https://archive.ics.uci.edu/ml/datasets/bank+marketing). The dataset was originally used in [1] and has more than 41,000 records from contacting customers in past telemarketing campaigns.


![#1589F0](https://placehold.it/15/1589F0/000000?text=+) **Problem overview:**

A bank wants to increase revenue by getting more customers to subscribe to long-term deposits. Based on data from past telemarketing campaigns, we want to predict which clients are more likely to subscribe, so that the bank can target these customers and improve their conversion rate. 

To show using cartoons, we basically want to know who will respond like the person on the left, and who will repond like the one on the right. :) 

![Cartoon](cartoon.png "Cartoon")


![#1589F0](https://placehold.it/15/1589F0/000000?text=+) **Folder structure:**

```bash
│   
├── codes
│   ├── baseline.ipynb (code for baseline performance)
│   ├── final.ipynb (modification on baseline.ipynb by clustering customers into different groups and by using cyclical feature engineering)
│    
|
├── data
│   ├── bank-additional-full.csv (original dataset)
│   ├── processed-train-baseline.csv (processed training dataset from baseline.ipynb)
|   ├── processed-test-baseline.csv (processed test dataset from baseline.ipynb)
│   ├── processed-train-final.csv (processed training dataset from final.ipynb)
|   ├── processed-test-final.csv (processed test dataset from final.ipynb)

```
If you just want to look at the code with the best results, see final.ipynb and the corresponding processed train and test datasets. The baseline code and datasets are kept to show how different feature engineering techniques helped improve the classifier performance.


![#1589F0](https://placehold.it/15/1589F0/000000?text=+) **Highlights:**

- The dataset is **imbalanced**, with only 11% of the customers called subscribing to long-term deposits. This means a majority classifier would easily get around 89% classification accuracy, but would be pretty useless as a predictor of which customers could be converted. So I used **ROC-AUC** (Area Under the ROC curve) instead of classification accuracy as a more effective metric for classifier performance.
- The features are a mix of **numerical** and **categorical** data. The features can be divided into 3 broad categories: **customer data** (age, marital status, job, etc.), **campaign data** (number of calls to the customer during this campaign, number of days since last contact, etc.) and **economic data** (employment variation rate, consumer price index, etc.).
- One of the features under **campaign data** is **duration**, which gives the call duration in seconds when the customer was last contacted. This feature highly affects the target variable, since intuitively a successfully converted customer would speak to the agent for a much longer duration about the investment details than a customer who is not interested. However, this feature is useless if the goal is to have a realistic predictive model, because:   
1. We would not know the call duration for a client in the present campaign until and unless the call is placed, i.e., this feature is unknown when we build our model.  
2. If we do wait until the end of the call to know the call duration, we would also know the outcome of the call (success/failure), so there is no point in having a machine learning model predict the outcome.  
Results using this feature would therefore be much better, but the model would not have real predictive power. So, this feature was discarded for all the analyses done in this work. 

![#1589F0](https://placehold.it/15/1589F0/000000?text=+) **Feature engineering, Models and Results:**

**Baseline code:**. 
 
 - To set up a baseline, I tried the following vanilla algorithms with default parameters: logistic regression, k-nearest neighbors, decision tree, random forest, and XGBoost classifiers. XGBoost and Logistic regression performed the best. 
 - With the top two candidates, I used Grid Search to find the best hyperparameters. XGBoost had the best ROC-AUC score.
 - With Recursive Feature Elimination, I selected the top 10 features using Random Forest classifier to see if this subset of features can perform better or as well as the full set of features. XGBoost still had the best performance, with a very slight drop in performance compared to the full feature set. But feature dimensionality was reduced from 33 to 10, considerably reducing training time and model complexity.
 
 **Modifications for the final code:**
 
 - From the feature selection procedure in the above code, 'month' and 'day_of_week' were seen to be among the top 10 features. Because they are ordinal categorical features, I had previously used ordinal encoding to encode them. However, they are also cyclic in nature (Monday repeats after Sunday and January repeats after December). So taking inspiration from [3], I tried using cyclic feature engineering to encode these two variables to see if it improves the performance. 
 
 - There are broadly 3 types of features in our dataset: **customer data**, **campaign data** and **economic data**. There are 7 features under customer data: 1 numeric (age), and the rest nominal categorical (job, marital status, education, default status, housing loan status and personal loan status). I wanted to see if it was possible to group customers into different clusters and create a new feature for each client depending on which cluster he/she falls into. This might be useful in several ways:  
 1. The number of features would go down, since we just have a cluster number for each customer instead of 7 features (22 after one-hot encoding) belonging to customer data. This would make training faster and our models less complicated.  
 2. It is possible that some of the features for some clients are noisy. Feeding these features directly to our models might hinder them from learning the right signals. Grouping similar customers together using unsupervised learning would smooth out such noisy data points.  
 3. If we know how different customer groups are likely to subscribe to long-term deposits, it would be easier for us to come up with different marketing strategies that are specifically targeted for each group, or maybe give more focus on high-potential customer groups to improve our conversion rate.
 
 Since features pertaining to customer data were of a mixed type (both numerical and categorical), I used k-Prototypes clustering [4] to come up with a new feature. Dropping the original features and introducing the new feature for customer group instead further boosted the ROC-AUC score slightly.
 
These two modifications jointly boosted the ROC-AUC score from 0.8111 to 0.815. While the improvement is not that much, the biggest advantage, according to me, is the fact that clustering brings down the number of features from 33 in the baseline code to just 14 in the final code. This is because most of the features belonging to customer data were nominal categorical in nature, having several possible values, so that one-hot encoding had expanded the feature set considerably. Clustering distilled all of this information into a single number, and even managed to produce a small performance improvement with it.
 
 My results, along with other results for this dataset I could find online, are tabulated below. Please note that most analyses with this dataset uses 'duration' as a feature, which easily gives an ROC-AUC score > 0.9. However as mentioned previously, since this feature is not known beforehand, we should not use it if our goal is to build models with real predictive power. As such, I have not included results that used this feature in the table below.
 
 Source | Train ROC-AUC | Test ROC-AUC 
--- | --- | --- 
Original paper [1] ** | -- | 0.794
[2] | 0.825 | 0.803
This work (baseline) | 0.8349 | 0.8111 (33 features)
This work (final) | 0.8199 | 0.8150 (14 features)

** According to the UCI dataset webpage, the dataset used in the original paper is very similar to the one currently in the UCI repository, but not exactly the same.

Further, I used Recursive Feature Elimination with Random Forest classifier to select a subset of 10 features in each of the 3 cases. Results using this feature subset are tabulated below.

Source | Train ROC-AUC | Test ROC-AUC 
--- | --- | --- 
10 features (baseline) | 0.8179 | 0.8074
10 features (final) | x | x

Although in both cases there is a slight drop in performance when compared to using all the features, feature dimensionality in the 3 cases was reduced to 10 from 33 and 14 respectively. The performance drop might be acceptable in view of the reduction in training time as well as model complexity that the reduced feature set offers. 


![#1589F0](https://placehold.it/15/1589F0/000000?text=+) **References:**. 

[1] Sérgio Moro, Paulo Cortez, and Paulo Rita. "A data-driven approach to predict the success of bank telemarketing." Decision Support Systems 62 (2014): 22-31. ([link](https://core.ac.uk/download/pdf/55631291.pdf))   
[2] Sukanta Roy. "Machine Learning Case Study: A data-driven approach to predict the success of bank telemarketing." Towards Data Science (Dec 2019). ([link](https://towardsdatascience.com/machine-learning-case-study-a-data-driven-approach-to-predict-the-success-of-bank-telemarketing-20e37d46c31c)).  
[3] David Kaleko. "Feature Engineering - Handling Cyclical Features." (Oct 2017). ([link](http://blog.davidkaleko.com/feature-engineering-cyclical-features.html)).  
[4] Zhexue Huang. "Clustering large data sets with mixed numeric and categorical values." Proceedings of the 1st pacific-asia conference on knowledge discovery and data mining,(PAKDD) 1997. ([link](https://grid.cs.gsu.edu/~wkim/index_files/papers/kprototype.pdf))
