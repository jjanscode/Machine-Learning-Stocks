## Introduction
It was observed that there is a robust community of swing traders and day traders on Twitter. A subset of these traders have built up a large following on the social media platform as a result of giving good recommendations on which stocks to buy and sell. The majority of the recommendations posted by these traders are volatile small cap and microcap stocks commonly referred to as "penny stocks".
Given that there are many different day traders with a large following on Twitter and each day trader discusses multiple different stocks simultaneously, the process of determining which equities to invest in was a time consuming task. Additionally, once an equity was purchased, it would need to be closely monitored for movements in price-- a process that consumed more time, and was subject to human factors (fear and greed). 

It was determined that in order to successfully operate in this area of equities, implementing a system with the following features would be necessary.  
  + Remove the human factors involved in trading these volatile equities.  
  + Remove the human factors involved in evaluating these equities.  
  + Greatly increase the volume of equities evaluated.  

## Methods
For this study, 150 Twitter accounts self identified as day and swing traders each with greater than 1,000 followers were followed. It was observed that generally on Sundays, this population of penny stock traders would discuss their picks for the week (equities they thought would perform well). Using the TweePy module, code for collecting the number of times a stock was mentioned on this timeline was written. The code was run late on Sunday nights, and then filtered to produce only a list of microcap and small cap stocks mentioned by the population on Sundays. More code was written to collect data and calculate features on short volume, trading volume and pricing for this filtered list of stocks, resulting in a total of 16 features included in the model. At the end of the trading week, two groups of classifier data was collected-- the first classifier being the maximum price movement above Monday opening price for a given equity, and the second classifier being maximum price movement below Monday opening price for a given equity. The classifiers were then matched with the volume, pricing, and Twitter features and fed into various different machine learning algorithms. 
![alt text](https://github.com/jjanscode/Machine-Learning-Stocks/blob/main/Block%20Diagram.png)  
Figure 1. Block diagram of the data collection and feature calculation process. 
