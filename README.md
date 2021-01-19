# Fintech-Project-2


![image](https://user-images.githubusercontent.com/69773959/104261580-1530e500-5443-11eb-956f-cb7af70136c6.png)


Our project is to create a trading algorithm, using Apple (AAPL) stock as a baseline. We'll fit a random forest algorithm to 5 years of AAPL stock market data to create a predictive model. We will also fit a LSTM and/or deep neural network model to the data and compare the performance of both models to determine which has the best predictive power to maximize potential portfolio value.

## Overview
* We are interested in stock market trading algotrthms

* We will use DataReader and yfinance (Yahoo Finance) to obtain our stock market data (AAPL)

* Machine learning models we expect to utilize are: 
   * Random Forest
   * Deep Neural Network
   * LSTM
   
* Statistical models we expect to explore are:   
   * Exponential Moving Average of Closing Prices
   * Exponential Moving Average of Daily Return Volatility
   * Bollinger Band
   * Exponential Smoothing

## Model Evaluation
* Ending portfolio values under each machine learning model will be used to evaluate the models


### Random Forest Trading
##### With the Random Forest, there is two different csv files. a model was loaded. The X_test is giving us data to used as we import the dump, load from joblib. By incorporating the apple stock data into our model to predict the y-values from the X-test values. 

![image](https://user-images.githubusercontent.com/69773959/105057971-fcbc5e00-5a32-11eb-9f03-b3df01466c3d.png)

![image](https://user-images.githubusercontent.com/69773959/105058029-0b0a7a00-5a33-11eb-9463-5290212a4329.png)


#### Next, we compare the predicted results to the actual results. Here we can see the positive returns, actual value, return and predicted value.  By looking at the first 20, it was determined from the predicted value there was 20 actual values, 9 positive returns and the best return date was 02/06/2018. When running the last 20, there was 20 actual returns, 10 positive returns, and the best date was 01/07/2021. 

![image](https://user-images.githubusercontent.com/69773959/105058242-3beaaf00-5a33-11eb-983f-fd3bbaa71305.png)

![image](https://user-images.githubusercontent.com/69773959/105058302-4d33bb80-5a33-11eb-9478-c0ae4068cc86.png)

#### We plotted the predicted results vs Actual results of the last 100 records. Here is determined the crossover signals, vol trend signal and Bollinger signal.

![image](https://user-images.githubusercontent.com/69773959/105058464-76ece280-5a33-11eb-866b-85f2552dc03c.png)

#### We plotted the the positive return vs the predicted value. 

![image](https://user-images.githubusercontent.com/69773959/105058533-8e2bd000-5a33-11eb-837a-c1374e2c403a.png)



![image](https://user-images.githubusercontent.com/69773959/105058589-9d128280-5a33-11eb-800e-6de0391555eb.png)



![image](https://user-images.githubusercontent.com/69773959/105058648-abf93500-5a33-11eb-9c59-e5fc069bb49d.png)


#### The random forest model predicts when to trade by giving accurate dates on positive returns.  By plotting the cumulative returns, we can see the trend for apple stock. In the end, the stock does drop, meaning the best time to trade and see a positive return will be 1/7/2021. 


## LSTM
#### We want to use a LSTM neural network to decide, given the openning stock price for each day of the test period, whether we are going to stay in the market for that day or not. We reshape the data (the LSTM wants the data in a particular shape, involving "windows") and at each step we want to predict the closing price of the day: in this way we will be able to find the vector v which selects the days during which we are going to stay in the market. For this model, we again use the same data to run our model. With this model, we start 50 days after the initial start (1/6/2016) and end before 1/5/2021. 
Now for each day we have the closing price for the day, the open price for the day (proxy for the closing price of the previous day) and the open price of the following day.

![image](https://user-images.githubusercontent.com/69773959/105059499-96383f80-5a34-11eb-94e2-215e8d03fd9d.png)

#### The feature rapp is the quotient between the open and closing prices of the day. It will be used because it gives the variation of the portfolio for the day. The gross yield was computed using the feature rapp.

![image](https://user-images.githubusercontent.com/69773959/105059591-b10ab400-5a34-11eb-9267-13465349ebe1.png)

#### We added columns corresponding to the moving average at 5 and 50 days. We then removed the 50 days since they do not have the 50 days moving average. 


![image](https://user-images.githubusercontent.com/69773959/105059680-ca136500-5a34-11eb-9fe6-f8618949404d.png)

![image](https://user-images.githubusercontent.com/69773959/105059710-d0094600-5a34-11eb-90e5-52f6167ee738.png)

#### We trained the data and start the model. This will help determine the train and test data.
![image](https://user-images.githubusercontent.com/69773959/105059813-ea432400-5a34-11eb-97ee-34f60062cd40.png)

#### Next, we can plot the predicted versus actual values. Notice that the predicted values are almost identical to the actual values; however, they are always one step ahead:

![image](https://user-images.githubusercontent.com/69773959/105060007-224a6700-5a35-11eb-891f-3d7facbb106b.png)

#### We stay in the market when the predicted price for the day's close is greater than the current day's opening price and stay out otherwise. The vector v indicates the "in days" (as 1s) and "out days" (as 0s)

![image](https://user-images.githubusercontent.com/69773959/105060113-3e4e0880-5a35-11eb-9d5f-88a342425dc3.png)

#### Now we can compare our LSTM-trading-strategy with the buy and hold strategy and the moving average strategy. In order to do so we compute the corresponding vectors v_bh and v_ma which select the months during which we are going to stay in the market.

![image](https://user-images.githubusercontent.com/69773959/105060209-5aea4080-5a35-11eb-8b36-ca8e41f512da.png)

##### We took the test period of 0.82. Buy and Hold looks like the best strategy when it comes to Total gross yield and Annual gross yield. 

![image](https://user-images.githubusercontent.com/69773959/105060306-76554b80-5a35-11eb-9c94-8fa5c1214b16.png)

## In conclusion, the Random Forest model gave us a date that was best to trade and have a positive return. While the LSTM mentioned the Apple stock was best to buy and hold. They are bnoth benefical to trade, making Apple a stock to invest and see an ROI. 
