# Fintech-Project-2


![image](https://user-images.githubusercontent.com/69773959/104261580-1530e500-5443-11eb-956f-cb7af70136c6.png)


Our project was to create a trading algorithm, using Apple (AAPL) stock as a baseline. We'll fit a random forest algorithm to 5 years of AAPL stock market data to create a predictive model. We will also fit LSTM and Support Vector Classifier (SVC) models to the data and compare the performance of all three models to determine which has the best predictive power to maximize potential portfolio value.

## Overview
* We are interested in stock market trading algotrthms

* We will use DataReader and yfinance (Yahoo Finance) to obtain our stock market data (AAPL)

* Machine learning models we expect to utilize are: 
   * Random Forest
   * Support Vector Classifier (SVC)
   * LSTM
   
* Statistical models we expect to explore are:   
   * Moving Average of Closing Prices
   * Exponential Moving Average of Closing Prices
   * Exponential Moving Average of Daily Return Volatility
   * Bollinger Band
   * Buy and Hold

## Model Evaluation
* Ending portfolio values/returns under each machine learning model will be used to evaluate the models


### Random Forest Trading
With the Random Forest, we create a trading signals CSV file based on (1) Exponential Moving Average of Closing Prices, (2) Exponential Moving Average of Daily Return Volatility and (3) Bollinger Bands. These signals are expressed as 1, 0 or -1:

![image](https://user-images.githubusercontent.com/69773959/105057971-fcbc5e00-5a32-11eb-9f03-b3df01466c3d.png)

Using these trading signals we then assign Training (75%) and Testing (25%) windows for our model and separate the X and y datasets accordingly. We then fit a SKLearn linear regression using just the training set (X_train, Y_train) and initiate and fit model.  The results are as follows:

![image](https://user-images.githubusercontent.com/69773959/105058029-0b0a7a00-5a33-11eb-9463-5290212a4329.png)

We then checked to see the relative importance of each of the three factos and checked the model's effectiveness:

![Model_Results(line41).png](Images/Model_Results(line41).png)

Next, we added the predicted results to the actual results' dataframe which can be seen as the following:

![image](https://user-images.githubusercontent.com/69773959/105058242-3beaaf00-5a33-11eb-983f-fd3bbaa71305.png)

By looking at the first 20, it was determined from the predicted value there was 20 actual values, 9 positive returns and the best return date was 02/06/2018. When running the last 20, there was 20 actual returns, 10 positive returns, and the best date was 01/07/2021. 

We then plotted the predicted results versus actual results:

![image](https://user-images.githubusercontent.com/69773959/105058302-4d33bb80-5a33-11eb-9478-c0ae4068cc86.png)

Looking at a plot of just the last 100 predicted results versus Actual results gives us the following:

![image](https://user-images.githubusercontent.com/69773959/105058533-8e2bd000-5a33-11eb-837a-c1374e2c403a.png)

At the end of the day, the usefulness of a predictive model is how well it does given new data.  Using the Random Forest Model to predict when to buy and sell provides the following returns based on Apple's stock price: 

![image](https://user-images.githubusercontent.com/69773959/105058648-abf93500-5a33-11eb-9c59-e5fc069bb49d.png)


### Long short-term memory (LSTM)
A second deep learning model we used was the Long short-term memory (LSTM) model.  LTSM is an artificial recurrent neural network (RNN) architecture and is well-suited to classifying, processing and making predictions based on time series data (such as stock price data).  Like the Random Forest model, we want to use a LSTM neural network to decide, given the openning stock price for each day of the test period, whether we are going to stay in the market for that day or not.  We reshape the data (the LSTM wants the data in a particular shape, involving "windows") and at each step we want to predict the closing price of the day: in this way we will be able to find the vector v which selects the days during which we are going to stay in the market. For this model, we again use the same data to run our model. With this model, as we use 5 and 50 day moving averages, we start 50 days after the initial start (1/6/2016) and end before 1/5/2021. 

Now for each day we have the closing price for the day, the open price for the day (proxy for the closing price of the previous day) and the open price of the following day.  From this data we derive the feature rapp, which is the quotient between the open and closing prices of the day. It is used to give us the variation of the portfolio for the day. The gross yield was computed using rapp.

![image](https://user-images.githubusercontent.com/69773959/105059499-96383f80-5a34-11eb-94e2-215e8d03fd9d.png)

To this dataframe we added columns corresponding to the 5 and 50 day moving averages. As mentioned previously, we then removed the first 50 days since they do not have the 50 days moving average. 

We want to use a LSTM neural network to decide, given the openning stock price for each day of the test period, whether we are going to stay in the market for that day or not.  We reshape the data (the LSTM wants the data in a particular shape, involving "windows") and at each step we want to predict the closing price of the day: in this way we will be able to find the vector v which selects the days during which we are going to stay in the market.  We then divided the dataframe into Train and Test sets (75/25) and defined the LTSM model.  We then run the model with 100 epochs and a batch size of 30.  Other epoch and batch size amounts were used but the significant increase in time was not worth the small incrmental decrease in 'val_loss'.

![image](https://user-images.githubusercontent.com/69773959/105059813-ea432400-5a34-11eb-97ee-34f60062cd40.png)

Next, we can plot the predicted versus actual values. Notice that the predicted values are almost identical to the actual values; however, they are always one step ahead:

![image](https://user-images.githubusercontent.com/69773959/105060007-224a6700-5a35-11eb-891f-3d7facbb106b.png)

We stay in the market when the predicted price for the day's close is greater than the current day's opening price and stay out otherwise. The vector v indicates the "in days" (as 1s) and "out days" (as 0s)

![image](https://user-images.githubusercontent.com/69773959/105060113-3e4e0880-5a35-11eb-9d5f-88a342425dc3.png)

Now we can compare our LSTM-trading-strategy with the both a buy and hold strategy and a moving average strategy (both 5-day and 50-day). In order to do so we compute the corresponding vectors v_bh and v_ma (short and long) which select the days during which we are going to stay in the market.

![image](https://user-images.githubusercontent.com/70610967/105270928-73e51580-5b4b-11eb-900b-3447618304cb.png)

We took a test period of 0.82. Taking a look at the results, Buy and Hold and 50-day Moving Average looks like the best strategy when it comes to Total gross yield and Annual gross yield. Although 5-day Moving Average is also quite promising, what is clearly evident is that the LTSM model does not create a superior trading model versus either Buy and Hold or trading based on moving averages.

![image](https://user-images.githubusercontent.com/70610967/105271114-cd4d4480-5b4b-11eb-8f74-b4042af7d235.png)


## Support Vector Machine (SVM)

Our third machine learning model evaluated was Support Vector Machine (SVM).  

In machine learning, SVMs are supervised learning models with associated learning algorithms that analyze data for classification and regression analysis.  Developed at AT&T Bell Laboratories, SVMs are based on statistical learning frameworks and are one of the most robust prediction methods.  Given a set of training examples, each marked as belonging to one of two categories, an SVM training algorithm builds a model that assigns new examples to one category or the other, making it a non-probabilistic binary linear classifier.  An SVM maps training examples to points in space so as to maximise the width of the gap between the two categories.  New examples are then mapped into that same space and predicted to belong to a category based on which side of the gap they fall.

Using the same set of Apple stock price data as the prior two models, we then determine the correct trading signal to determine when to buy or sell Apple's stock.  We will store +1 for buy signal and -1 for sell signal in the Signal column. 'y' is a target dataset storing the correct trading signal which the machine learning algorithm will try to predict.  The X is a dataset that holds the variables which are used to predict y, that is, whether Apple stock price will go up (1) or go down (-1) tomorrow. The X consists of variables such as 'Open - Close', 'High - Low' and 'Volume'. These can be understood as indicators based on which the algorithm will predict tomorrow's trend.

![image](https://user-images.githubusercontent.com/69773959/105270274-be1dc500-5b52-11eb-8609-6dad5654f96f.png)

We then use the SVC() function from sklearn.svm.SVC library for the classification and create our classifier model using the fit() method on the training data set.

Similar to the Random Forest model, we compute the accuarcy of the algorithm on the train and test data set, by comparing the actual values of Signal with the predicted values of Signal.  An accuracy of 50%+ in test data suggests that the classifier model is effective.

![image](https://user-images.githubusercontent.com/69773959/105270603-48662900-5b53-11eb-8f2f-9b45a9195a48.png)

We then predict the signal (buy or sell) for the test data set, using the cls.predict() fucntion and compute the strategy returns based on the predicted signal, saving it in the dataframe column 'Strategy_Return' and plot the cumulative strategy returns.

![image](https://user-images.githubusercontent.com/69773959/105270777-95e29600-5b53-11eb-8668-532d6f0a18ae.png)

As seen from the above graph, the strategy generates a return of 75%+ for the test data set.

#### In conclusion, the Random Forest model gave us the best positive returns, closely followed by SVC. The LSTM model was the weakest and produced returns far below just a buy and hold strategy. 
