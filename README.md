This repository contains codes and data for the bank marketing dataset available on [Kaggle](https://www.kaggle.com/volodymyrgavrysh/bank-marketing-campaigns-dataset) and on [this UCI repository](https://archive.ics.uci.edu/ml/datasets/bank+marketing).

**Problem overview:**
A bank wants to increase revenue by getting customers to subscribe to long-term deposits. Based on data from past telemarketing campaigns, we want to predict which clients are more likely to subscribe, so that the bank can target these customers and improve their conversion rate.

**Folder structure:**

```bash
├── data
│   ├── bank-additional-full.csv (original dataset)
│   ├── bank-additional-full-processed.csv (processed dataset)
│   
├── codes
│   ├── 1_data_preprocessing.ipynb (codes for EDA and data preprocessing)
│   ├── 2_models.ipynb (builds predictive models using processed dataset)
│    
```

**Feature engineering:**

- The dataset is heavily **imbalanced**, with only 11% of the customers called subscribing to long-term deposits. 
- The features are a mix of **numerical** and **categorical** data, which can be broadly divided into **customer data** (age, marital status, job, etc.), **campaign data** (number of calls to the customer during this campaign, number of days since last contact, etc.) and **economic data** (employment variation rate, consumer price index, etc.).

**Model-building:**
 
 - To set up a baseline, I tried the following vanilla algorithms with default parameters: logistic regression, decision tree, random forest, gradient boosting classifier and k-nearest neighbor. 

**Results:**

**Insights into customer trends:**
