## Introduction
It was observed that there is a robust community of swing traders and day traders on Twitter. A subset of these traders have built up a large following on the social media platform as a result of giving good recommendations on which stocks to buy and sell. The majority of the recommendations posted by these traders are volatile small cap and microcap stocks commonly referred to as "penny stocks".
Given that there are many different day traders with a large following on Twitter and each day trader discusses multiple different stocks simultaneously, the process of determining which equities to invest in was a time consuming task. Additionally, once an stock was purchased, it would need to be closely monitored for movements in price-- a process that consumed more time, and was subject to human factors (fear and greed). 

It was determined that in order to successfully operate in this area of equities, implementing a system with the following features would be necessary.  
  + Remove the human factors involved in trading these volatile equities.  
  + Remove the human factors involved in evaluating these equities.  
  + Greatly increase the volume of equities evaluated.  

## Data Collection Methods and Machine Learning Model Feature Description
For this study, 150 Twitter accounts self identified as day and swing traders each with greater than 1,000 followers were followed. It was observed that generally on Sundays, this population of penny stock traders would discuss their picks for the week (stocks they thought would perform well in the upcoming week). Using the TweePy module, code for collecting the number of times a stock was mentioned on this timeline was written. The code was run late on Sunday nights, and then filtered to produce a list of microcap and small cap stocks mentioned by the population on Sundays. More code was written to collect data and calculate features on short volume, trading volume and pricing for this filtered list of stocks. These automatically calculated features were combined with additional macroeconmic indicators and weekly purchasing data, resulting in a total of 16 features included in the model (Table 1). At the end of the trading week, two groups of classifier data was collected-- the first classifier being the maximum price movement above Monday opening price for a given stock, and the second classifier being maximum price movement below Monday opening price for a given stock (Table 2). The classifiers were then matched with the volume, pricing, and Twitter features and fed into various different machine learning algorithms.
![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Block%20Diagram.png)  
Figure 1. Block diagram of the data collection and feature calculation process. 
  
    
![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Feature%20Summary.png)  
Table 1. Summary of features included in machine learning model.  
Table 2. INSERT CLASSIFIER TABLE!!!!!!!!!!!!!!!!!!!!!  
  
  
  
  
## Classifier Data
!!!!!!!!!INSERT CLASSIFIER DESCRIPTOR TABLE 
The microcap and small cap stocks mentioned Sunday night by the population of day traders proved to have large weekly price movements. The median maximum weekly price movement above Monday opening price was 8.29% (Figure 3) and the median maximum weekly price movement below Monday opening price was 9.09% (Figure 4) across an n = 712 dataset collected between 7/26/2020 and 12/13/2020 (source file:'Machine Learning Set V2.xlsx').  

![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Max%20Up%20Classifiers.png)   
Figure 3. Weekly maximum price classifier histogram for data set. Calculated with the file 'DataHistograms.ipynb'.  
Note: 40% bin represents all weekly classifiers that are 40% or greater.  

![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Max%20Down%20Classifiers.png)   
Figure 3. Weekly minimum classifier histogram for data set. Calculated with the file 'DataHistograms.ipynb'.  
Note: 40% bin represents all weekly classifiers that are 40% or greater.  
  
  
  
  
## Machine Learning Algorithm Performance
Upon investigation of various machine learning algorithms available in the Sci-Kit Learn Python package, it was determined that a random forest algorithm performed the best at both accuractely classifying the data and minimizing overfitting.  
The random forest algorithm incorporated n=100 estimators and methods to control for overfitting. Only 8 out of the 16 features from the machine learning set were used to train a given estimator, and the minimum number of samples allowed in a leaf of the tree was set to 15.  
In order to systematically determine the performance of the random forest algorithm, two separate sensitivity sweeps were performed across the two classifiers (maximum price movement and minimum price movement).  
In this study, 18 different classifier thresholds between 0 and 41% were established for both the maximum price movement and minimum price movement classifiers. For each classifier threshold a binary classification system was used to label the data, with a '1' indicating the classifier for the data point was above the classifier threshold, and a '0' indicating the classifier for the data point was below the classifier threshold.  
To establish a robust metric of algorithm performance at a given classifier level, the random forest algorithm was trained 100 separate times using a different random seed to split the data set into training and testing groups. The training and testing accuracy of the algorithm across the 100 iterations was tracked (Figure 4).  
![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Four%20Down%20Random%20Seed%20Sweep.png)  
Figure 4. Training and testing accuracy of a 4% thresholded minimum price classifier across 100 different random seeds (associated code file: Machine Learning Stocks Sensitivity Testing.ipynb).  
  
The median training accuracy, testing accuracy, standard deviation in training accuracy, and standard deviation in testing accuracy for the random forest algorithm across the 18 classifier thresholds for both types of classifiers were computed and plotted (Figure 5, Figure 6).  
![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Max%20Up%20Sensitivity.png)  
Figure 5. Median maximum price movement classifier threshold sensitivity sweep using an n=100 estimators random forest machine learning algorithm trained using 100 different random seeds.  
  
![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Max%20Down%20Sensitivity.png)  
Figure 6. Median training and testing accuracy of the minimum price movement classifier across thresholds betwen 0% and 41% using an n=100 estimators random forest machine learning algorithm trained using 100 different random seeds. 
  
  
