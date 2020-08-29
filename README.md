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

- The dataset is **imbalanced**, with only 11% of the customers called subscribing to long-term deposits. This means a majority classifier would easily get around 90% classification accuracy, but would be pretty useless. So I used ROC-AUC (Area Under the ROC curve) instead of classification accuracy as a more effective metric for classifier performance.
- The features are a mix of **numerical** and **categorical** data, which can be broadly divided into **customer data** (age, marital status, job, etc.), **campaign data** (number of calls to the customer during this campaign, number of days since last contact, etc.) and **economic data** (employment variation rate, consumer price index, etc.).

**Feature engineering:**

**Model-building:**
 
 - To set up a baseline, I tried the following vanilla algorithms with default parameters: logistic regression, decision tree, random forest, gradient boosting classifier and k-nearest neighbor. 

**Results:**

**Insights into customer trends:**
