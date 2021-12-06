Algorithmic high-speed trading systems are being implemented in all finacial markets, especially in the cryptocurrency market due to its trading operations happen 24/7 over the world without discontinuaty and it has been the most computerized market so far. Investors are from governments, big institutions to individuals. This experiment tried test the performance of a deep learning model that can be used as a part in a trading system.

# To Recreate the Experiment
1. Create an account on Binance exchange to access its API then get API key and security key.
2. Assign API key and security key to their variables in binance_key.ipynb file.
3. Run retrieve_data.ipynb file to down the historiral data.
4. Run main_code.ipynb file to process data, construct, train, and test the model. Full code can be viewed [HERE](https://github.com/ba-luan/data_science_journal/blob/main/Crytopcurrency%20Price%20Prediction%20in%20Small%20Time%20Intervals/main_code.ipynb) 

# Overview
### Feature Engineer
The original idea was the model collecting data of the whole market to make trading decisions on a specific coin or token. A market represented index usually used as a solution such as S&P50 in the U.S. stock market. In the experiment, the top 4 coins from four different segments of the cryptocurrency market were selected as a represented index (all treated with the same weight):
- Bitcoin (BTC) - the Store of Value segment
- Ethereum (ETH) - the Smart Contract segment
- RippleX (XRP) - the Payment solution segment
- UniSwap (UNI) - the Decentralized Finance (DeFi) segment

Responses received from the Binance API under JSON format including many infomation but only OHLC prices (Open, High, Low, Close) and trading volume were obtained.

There were inconsistencies in the value ranges of all features. For example, in 1 minute, Bitcoin price ranging in thousands dollars and its trading volume in dozens of BTC; in the other hand, RippleX price ranging in cents but its trading volume up to hundreds of thousands of XRP. Feature values were normalized by the Z-normalization method.
* new value = (old value – moving average) / standard deviation

### Constructing Model
Transformed to Classification model, not Regression model – predicting “buy or not buy” signals instead of numeric values.
Labels assigned at every timestep in the dataset:
- not buy
-  buy now and sell after n minutes

There were 2 types of labeling method:
-  Fixed-n step: e.g., not buy or buy now and sell after 2 minutes
-  Max-n step: e.g., in the next 2-5 minutes, the 3rd minute has highest price, buy now and sell after 3 minutes

Pre-built layers in TensorFlow Keras library used to construct the neural network: stacked multiple Long Short-Term Memory (LSTM), Dense, and Dropout to increase randomness and prevent overfitting.

Optimizing model by hyperparameters tuning process. Experiment with three hyperparameters in the model:
- Number of units (neurons): 32 and 64
- Dropout rate: .2 and .5
- Optimizers: Stochastic Gradient Descent (SGD) and Adaptive Moment Estimation (Adam)

The optimized model predicted trading signals for 4 cryptos in 2 labeling methods:
- Bitcoin, Ethereum, RippleX, UniSwap
- Fixed-n timestep and Max-value timestep

# Conclusion
Performance was evaluated by the accuracy and the maximum thresholds achieved:
- 53.5% in fixed-2-minutes timestep (7% higher than random guess)
- 22.5% in max-value of 2-5 minutes window (12.5% higher than random guess and with the maximum price increase)

The accuracy rates can be compensated by the advantages of a high-speed trading system that accumulates small gains

# Future Works
- Deploy the model for real-time trading with a budget
- Consider new performance evaluating metrics (ROI or loss-reward ratio instead of accuracy)
- Consider trading infomation (spread, commission, trading size)
- Add more cryptos into the represented index (top30 coins as Dow Index)
- Use different type of moving average (Exponential, Smoothed, or Linear Weighted instead of Simple)
- Add trading indicator (Relative Strength Index, Stochastics, MACD, Bollinger Bands…)


