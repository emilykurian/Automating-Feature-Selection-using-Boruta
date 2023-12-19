# Automating-Feature-Selection-using-Boruta
Aims to select the most informative and impactful subset of features using Boruta algorithm, aiming to enhance model performance, reduce computational overhead, and improve interpretability.    

Real world datasets have a tremendous number of columns or features, which makes the data modeling complex and highly computational. As datasets grow in complexity and dimensionality, selecting the most informative and impactful subset of features becomes a crucial step in building effective models. Feature selection involves identifying and retaining a subset of features from the original set, aiming to enhance model performance, reduce computational overhead, and improve interpretability.  

The abundance of features in a dataset often includes both relevant and irrelevant information, posing challenges such as overfitting, increased computational demands, and decreased model interpretability. Through feature selection, practitioners seek to distill meaningful insights from the data, focusing on the features that contribute most significantly to the predictive power of the model.  

Boruta is a powerful and versatile feature selection algorithm designed to uncover the most relevant and informative features within a dataset.Introduced as an extension to the Random Forest algorithm, Boruta operates by assessing the importance of each feature in relation to a target variable. Unlike traditional feature selection methods that rely solely on statistical measures, Boruta takes a model-driven approach. It creates shadow or "randomized" features alongside the original features, ensuring that the algorithm evaluates the significance of each variable against its randomized counterpart. The maximum feature importance among the shadow features serves as a threshold. Of the original features, only those whose importance is above this threshold score a point. In other words, only features that are more important than random vectors are awarded points.   

All the other methods of feature selection require a human to make an arbitrary decision. Unsupervised methods need us to set the variance or VIF threshold for feature removal. Wrappers require us to decide on the number of features we want to keep upfront. Filters need us to choose the correlation measure and the number of features to keep as well. Embedded methods have us select regularization strength. Boruta needs none of these.  

# Methodology 
## PART 1
**1. Data  pre-processing:**  
* The data is pre-processed to remove missing values and further cleaning like converting categorical values into numerical, etc..
  
**2. Creation of Shadow (Randomized) Features:**   
* For each feature in the original dataset, Boruta generates a corresponding shadow feature.  
* Shadow features are created by shuffling the values of the original features, maintaining the same distribution but eliminating the genuine relationships with the target variable.    
* The original features and their shadow counterparts are combined into a single dataset for further analysis.
  
**3. Random Forest Feature Importance:**  
* Combined dataset is used to train a Random Forest model.  
* _Feature_importances__ (measure used is Gini impurity or mean decrease accuracy) gives the importance score of each original feature and shadow feature.
  
**4.Feature selection:**  
* Retrieves the importance score of the most important shadow feature. This serves as a benchmark for selecting important original features.  
* Original features whose importance scores exceed the importance of the most important shadow feature.    
* Conducts multiple trials of the feature selection process and counts how many times each feature is selected as important across the trials.  

## PART - 2: Using binomial distribution to find the important among the above retrieved

**1. Calculate the Probability Mass Function (PMF):**
* The probability of each possible number of successes in the binomial distribution is obtained.
  
**2. Identify the Tail Items in the Distribution:**
* 5% of the overall probability in the tails of the binomial distribution.  
* Calculates the index at which the cumulative sum of the PMF reaches or exceeds 5% or in other words number of iterations that form the tail. This index is the threshold.  
* Features that consistently appear as important in fewer than threshold value out of the total trials are considered less stable or less frequently selected as important.  
* Green zone is when it appears important in greater than or equal to Total-threshold times.  
* In between threshold and total - threshold is blue zone.  
  
Example if thresh = 19 for trials = 50  
Then green zone if >= (50-19) = 31. indicating high stability.  
Blue zone if between 19 and 31, indicating moderate stability.  

![image](https://github.com/emilykurian/Automating-Feature-Selection-using-Boruta/assets/49084104/a48119d1-23d4-437e-8475-e268e44a18e7)



