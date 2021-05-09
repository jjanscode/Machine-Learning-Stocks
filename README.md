## Introduction
It was observed that there is a robust community of stock traders on Twitter. A subset of these traders are known as day traders and swing traders-- traders who buy and sell stocks on a short term time scale of typically around a couple of days. Some of these traders have built up a large following on the social media platform as a result of being percieved to give good recommendations on stocks. The majority of the recommendations posted by these traders are volatile small cap and microcap stocks sometimes referred to as "penny stocks". These penny stocks experience large short term price movements yielding large profits or losses over the course of several days, relatively independent of larger macroeconomic trends. 

Each individual trader in this population discusses multiple different stocks simultaneously and from qualitative observation, any one given trader was not 100% correct in their recommendations. Weighing multiple recommendations made by many different traders and determining the validity of the recommendations represented an overwhelming amount of information for one user to digest, and this process was subject to human biases. Additionally, once a decision on which stocks to purchase were made, the purchased stocks would need to be closely monitored for movements in price-- a process that consumed more time, and was subject to more human factors. 

It was determined that in order to successfully operate in this area of stocks, implementing a rigid system to process and make decisions on this large amount of recommendations would be necessary. It was determined that implementing a machine learning model with associated automated data collection code would: 
  + Remove the human factors involved in evaluating these stocks.    
  + Remove the human factors involved in trading these stocks.  
  + Greatly increase the volume of stocks evaluated.  

## Data Collection Methods and Machine Learning Model Feature Description
For this study, 150 Twitter accounts self identified as day and swing traders each with greater than 1,000 followers were followed. It was observed that generally on Sundays this population of penny stock traders would discuss and recommend stocks they thought would perform well in the upcoming week. Using the TweePy module, code for collecting the number of times a stock was mentioned on this Sunday night timeline was written. The code was run late on Sunday nights and the resulting list was filtered to exclude stocks that were not available for trading on the WeBull stock trading application, were currently priced higher than $40, or had a 52 week minimum price greater than $10. This produced a list of mostly microcap and small cap stocks mentioned by the trader population on Sunday nights. More code was written to collect data and calculate features on short volume, trading volume and pricing for this filtered list of stocks. The automatically calculated features were combined with additional macroeconmic indicators and weekly purchasing data, resulting in a total of 16 features in the machine learning model (Table 1).  
  
![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Feature%20Collection%20Block%20Diagram.png)  
__Figure 1. Block diagram of the data collection and feature calculation process.__ 
  
    
    
    
![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Feature%20Summary.png)  
__Table 1. Summary of features included in machine learning model.__  

  
  
  
  
## Classifier Data
 At the end of the trading week, two separate groups of classifier data were collected--  maximum price movement above Monday opening price for a given stock (maximum price movement classifier), and the minimum price movement below Monday opening price for a given stock (minimum price movement classifier) (Table 2). The classifiers were then matched with the associated 16 features and were used to train various different machine learning algorithms. 

 ![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Classifier%20Table.png)  
__Table 2. Summary of classifier sets included in machine learning model.__  
  
  
The microcap and small cap stocks mentioned Sunday night by the population of day traders proved to have large weekly price movements. The median maximum weekly price movement above Monday opening price was 8.00% (Figure 3) and the median maximum weekly price movement below Monday opening price was 10.23% (Figure 4) across an n = 1758 dataset collected between 7/26/2020 and 4/25/2021 (source file:'Machine Learning Set V2.xlsx').  
  

![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Max%20Up%20Classifiers.png)   
__Figure 3. Weekly maximum price classifier histogram for data set. Calculated with the file 'DataHistograms.ipynb'.  
Note: 40% bin represents all weekly classifiers that are 40% or greater.__

![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Max%20Down%20Classifiers.png)   
__Figure 4. Weekly minimum classifier histogram for data set. Calculated with the file 'DataHistograms.ipynb'.  
Note: 40% bin represents all weekly classifiers that are 40% or greater.__  
  
  
  
  
## Machine Learning Algorithm Performance
Upon investigation of various machine learning algorithms available in the Sci-Kit Learn Python package, it was determined that a random forest algorithm performed the best at both accuractely classifying the data and minimizing overfitting.  
The random forest algorithm deployed incorporated n=100 estimators. To control for overfitting, only 8 out of the 16 features from the data set ('Machine Learning Set V2.xlsx') were used to train a given estimator, and the minimum number of samples allowed in a leaf of the tree was set to 15.  
In order to systematically determine the performance of the random forest algorithm, a sensitivity sweep was performed across the two classifiers (maximum price movement and minimum price movement).  
In this sensitivity study, 18 different classifier thresholds between 0 and 41% were established for both the maximum price movement and minimum price movement classifiers. A binary classification system was used to label the data at each threshold, with a '1' indicating the classifier for the data point was above the classifier threshold, and a '0' indicating the classifier for the data point was below the classifier threshold.  
To establish a robust metric of algorithm performance at a given classifier level, the random forest algorithm was trained 100 separate times using a different random seed to split the data set into training and testing groups. The training and testing accuracy of the algorithm across the 100 iterations was tracked (Figure 5).  
![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Four%20Down%20Random%20Seed%20Sweep.png)  
__Figure 5. Training and testing accuracy of a 4% thresholded minimum price classifier across 100 different random seeds (associated code file: Machine Learning Stocks Sensitivity Testing.ipynb).__
  
The median training accuracy, testing accuracy, standard deviation in training accuracy, and standard deviation in testing accuracy for the random forest algorithm across the 18 binary thresholds for both classifier sets were computed and plotted (Figure 6, Figure 7).  
__![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Max%20Up%20Sensitivity.png)  
Figure 6. Median maximum price movement classifier threshold sensitivity sweep using an n=100 estimators random forest machine learning algorithm trained using 100 different random seeds.__ 
  
__![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Max%20Down%20Sensitivity.png)  
Figure 7. Median minimum price movement classifier threshold sensitivity sweep using an n=100 estimators random forest machine learning algorithm trained using 100 different random seeds.__ 
   
  
   
From this sensitivity test, it was determined that machine learning models trained on binary classifier thresholds between 0-4% and 17+% produced acceptable accuracies, whil not badly overfitting the data given the relatively small differences in training and testing accuracy (Figure 6, Figure 7).

## Feature Analysis
To gain further insight into the importance of the various features included in the machine learning model, a sensitivity sweep of machine learning feature importance scores was performed for both the Max Up and Max Down classifier sets. A feature importance score is a decimal number between 0 and 1 produced by the machine learning model that indicates how important a given feature is in discriminating between the different groups of the classifier data. The feature importance scores for all of the features sum up to 1, with higher scores indiciating more importance. For this sensitivity test, each machine learning algorithm was trained 100 times with different random seeds each iteration, and the median feature importance scores were selected.   
  __![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/First%20Four%20Max%20Up%20Features.png) 
 Figure 8. Feature importance sensitivity sweep for first four machine learning model features across maximum weekly upwards price movement classifier thresholds.__  
 
  __![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Second%20Four%20Max%20Up%20Features.png) 
 Figure 9. Feature importance sensitivity sweep for second four machine learning model features across maximum weekly upwards price movement classifier thresholds.__  
 __![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Third%20Four%20Max%20Up%20Features.png)
  Figure 10. Feature importance sensitivity sweep for third four machine learning model features across maximum weekly upwards price movement classifier thresholds.__  
 
 __![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Last%20Four%20Max%20Up%20Features.png)
 Figure 11. Feature importance sensitivity sweep for last four machine learning model features across maximum weekly upwards price movement classifier thresholds.__  
 
  __![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Max%20Down%20First%20Four%20Features.png)
Figure 12. Feature importance sensitivity sweep for first four machine learning model features across maximum weekly downwards price movement classifier thresholds.__  
  __![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Max%20Down%20Second%20Four%20Features.png)
 Figure 13. Feature importance sensitivity sweep for second four machine learning model features across maximum weekly downwards price movement classifier thresholds.__  
  __![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Max%20Down%20Third%20Four%20Features.png)
 Figure 14. Feature importance sensitivity sweep for third four machine learning model features across maximum weekly downwards price movement classifier thresholds.__   
  __![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Max%20Down%20Lasat%20Four%20Features.png)
  Figure 15. Feature importance sensitivity sweep for last four machine learning model features across maximum weekly downwards price movement classifier thresholds.__    
 
 ## Implementation and Performance
The best methods for assessing forward looking predictions made by trained forest algorithms and making purchasing decisions is being investigated. The current approach integrates predictions made from 16 machine learning algorithms separately trained on differently thresholded classifier data into a scoring scheme (Table 3). In this scoring scheme, the 16 different algorithms are fed a matrix of stocks with the associated feature data and are asked to make a prediction on each stock. The score for a stock initializes at a value of zero, if a given machine learning algorithm predicts the stock to be above the classifier threshold it was trained on, points equal to the testing accuracy of the given algorithm are added to the stock score. This process is repeated for all 16 algorithms and all stocks in the weekly dataset. All of the machine learning stock scores are then normalized by the maximum possible score to produce a normalized percentile for easy week to week comparison. 
  __![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Points%20Scheme.png)  
Table 3. Summary of machine learning based scoring system, associated code file: Machine Learning Scoring.ipynb__  
   
   
   

Additional scoring schemes are expected to incorporate factors for overfitting, and different permutations of rules for adding/subtracting/no action points scoring. These scoring schemes will undergo an evaluation period of a month, during which the machiune learning percentile prediction versus weekly stock performance for each scheme will be evaluated. Trends identified during this period will be used to to formulate a rules based approach for buying and selling equities. Different rules based approaches will be evaluated using the yfinance Python module on minutely price data to simulate deploying the buying and selling paradigm in real time (Figure 8). 
 ![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Github%20Machine%20Learning%20Images/Entire%20Block%20Diagram.png)  
__Figure 8. Summary of the entire data collection and evaluation process.__  

 
In addition to this ongoing work, planned future work will include the following investigations.  
  + Perform sensitivity analysis of feature importance scores across binary classifier thresholds for both classifier sets, investigate the removal of features determined to be universally unimportant.       
  + Continue working to automate all features of data collection process.   
  + Investigate establishing stricter stock filtering criteria so as to create a more homogenous dataset. Investigate the impact of this on machine learning model performance. 
  + Revisit evaluating the performance of the random forest algorithm on multiclass labeled data. 

