# ArimaProject
## <h2> Overview
In the context of the strong development of the digital age, besides the financial markets that are too familiar to investors such as the stock market, the forex market,... The birth and explosion of Blockchain technology have produced the first coin - Bitcoin and from that milestone until now, the cryptocurrency market is growing strongly with the advent of hundreds of other cryptocurrencies and then gradually gaining a foothold in the market. And as an obvious consequence, the cryptocurrency market is growing in popularity and proving its worth, through the benefits it brings. Cryptocurrencies have a strong advantage over the decentralized financial system - which is supposed to replace traditional centralized finance. The potential that cryptocurrencies bring is really great both now and in the future. Therefore, the author chooses cryptocurrencies as the main financial asset for this topic. 

Back to the crypto market for trading purposes, nowadays, the coin is gradually becoming an asset in investors' portfolios. However, accompanied by high returns, the coin is still a risky portfolio because of its high volatility. So, this project aims to build a web application to forecast the trend of coin prices using the Arima model - a fairly famous model in time series forecasting (This project is not aimed at a specific coin, but at all tradable coins on the market).
	
## <h2> Data processing
The content of this chapter is handled in the file ApiGetData.py 

The data was crawled from API of Coinbase with url is  https://api.pro.coinbase.com (No key needed like some other sites). Data retrieved by date includes attributes as Open, Close, High, Low and Volume, and price of coin based on USD. By setting params to request as start time, end time and barsize, input is symbol of coin with return json format, we will get the data directly from Coinbase. However, the limitation of this API is that it can only crawl 300 records at a time, so the author must use a While loop to crawl all possible data of a coin. Then, format data converts the data type from json to DataFrame, we will get data.

In addition, to diversify the time type and predict in the long term, the data is also formatted from days to a week, two weeks, and a month depending on the input requirements. This means that users will have four data options: day, week, fortnight and month depending on the purpose. And for the purpose of the topic based on many coins, and to avoid errors in the data entry step, the project also gets the list of coins from Coinbase, via url https://api.pro.coinbase.com/currencies. 
Finally, the data is retrieved quite clean,  the required data is quite complete and there is no sign of error, through the above steps, the data is ready to display and feed into the model.
	
## <h2> Arima model and App web 
## <h3> Arima model
The content of this section is handled in the file ArimaModel.py 
## <h4> Introduce
An autoregressive integrated moving average, or ARIMA, is a statistical analysis model that uses time series data to either better understand the data set or to predict future trends. The ARIMA model includes the following input variables (p, d, q):
- Autoregression (AR) (p): refers to a model that shows a changing variable that regresses on its own lagged, or prior, values.
- Integrated (I) (d): represents the differencing of raw observations to allow for the time series to become stationary (i.e., data values are replaced by the difference between the data values and the previous values).
- Moving average (MA) (q):  incorporates the dependency between an observation and a residual error from a moving average model applied to lagged observations.
## <h4> Perform
The project uses the Close column data as input. However, considering that for coin price data, they tend to be future-specific, which means they are non-stationary. So, to ensure the stationarity of the input data, the project uses the closing price column profit (symbol r_t) through the formula:
- r_t=log⁡(x_t/x_(t-1) )\
	
Then the model will predicts the next series of r_t and returns the actual price. 
Because the purpose of building the app is to predict many coins, it is not possible to input the default parameters (p,d,q) because of the different characteristic of each coin. In addition, it can be seen through a simple strategy that the selection of the best model is based on the AIC index. The project will therefore automate this process using auto_arima. For input variables, the author chooses d=0 because the r_t series has guaranteed stationarity. Parameters q and p will be in the range (1,10). In addition, to optimize the process. Through the above steps, we have built a specific model for each coin and can be used for prediction.
## <h3> App Web
The content of this section is handled in the file StreamlitApp.py 
After completing the data processing and model building steps, the next step will be to build the app. The project uses Streamlit library in python. Streamlit is an open-source app framework for Machine Learning and Data Science teams. 
In this app, you can select your desired coin and period in the sidebar, then view the data and graph. Select the period you want to forecast and press the Predict button, the app will return the model and forecast results for you. You can experience it through the following link: https://share.streamlit.io/hoangkim133/arimaproject/main/StreamlitApp.py
> Note: Over time if inactive, the link may die and become inoperable. If that's the case, please run the StreamlitApp.py file and experience it on localhost (Or contact to author to reboot the link).

## <h2> Results and Conclusion <h2> 
Although each coin gives a different set of parameters (p,d,q) resulting in different Arima models. But through general observation, it can be seen that the upper and lower margins of the price fluctuate quite strongly, which can lead to confusion in transactions. For the P-values of the coefficients, with a 95% confidence level, there are occasional coefficients that exceed this level. In addition, the author also found that when predicting the future over a large number of periods, prices will become out of control and unreasonable. Therefore, the author has limited the prediction period to the interval (1, 5). But if investors want to predict in the long term, they can choose data type by week, two weak or month. After all, investors can observe the forecast line (red line) to determine the trend and make decisions.

In summary, the purpose of this project is to provide a tool for investors to refer to, but it is not recommended to completely trust the model's predictions. Surely the project still has a lot of points to improve in terms of modeling, or there may be some bugs in the code (some coin cannot get data) or some other hidden bugs that the author has not discovered yet. After all, the project will try to overcome the weaknesses, try different methods to perfect the model and predict the time series, on the other hand, improve the interface to enhance the investor experience.
	
## Reference:
- Khoa học dữ liệu – Khanh’s blog
- Quang, B. (2010). Ứng dụng mô hình ARIMA để dự báo VNINDEX.


