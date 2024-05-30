# Stock-Market-Performance-Analysis-using-Python

## About

Stock Market Performance Analysis This project involves calculating moving averages, measuring volatility, conducting correlation analysis, and analyzing various aspects of the stock market to gain a deeper understanding of the factors that affect stock prices and the relationships between the stock prices of different companies.

## Overview

Stock market performance analysis can be used to inform investment decisions and help investors make informed decisions about buying or selling stocks. Suppose you work as a data science professional in a company that provides services based on investment decisions. As a data science professional, you can help your business by analyzing the historical performance of different companies, identifying potential opportunities and risks in the stock market, and adjusting your clients’ investment strategies accordingly.

As a data science professional, you can go through a structured process of stock market performance analysis, which involves collecting historical stock price data of different companies from trusted sources such as Yahoo Finance, visualizing data using various charts, calculating movements, averages and volatility for each company, and performing correlation analysis to analyze the relationships between different stock prices.

In the section below, I will take you through the task of Stock Market Performance Analysis using Python step by step.

Stock Market Performance Analysis using Python
Let’s start the task of Stock Market Performance Analysis by importing the necessary Python libraries and the dataset. For this task, I will use the Yahoo finance API (yfinance) to collect real-time stock market data for the past three months.

It’s important to collect real-time data for this task, but still, if you are a complete beginner and want a dataset only to practice the concepts covered in this article, you can download the dataset from here. But it’s recommended to use the yfinance API to collect and work on real-time data. You can install the yfinace API in your Python environment using the pip command mentioned below (run the command below on your command prompt or terminal):

for command prompt or terminal: pip install yfinance
for Google Colab or Jupyter notebooks: !pip install yfinance
Now below is how we can collect real-time stock market data using the yfinance API:

import pandas as pd
import yfinance as yf
from datetime import datetime

start_date = datetime.now() - pd.DateOffset(months=3)
end_date = datetime.now()

tickers = ['AAPL', 'MSFT', 'NFLX', 'GOOG']

df_list = []

for ticker in tickers:
data = yf.download(ticker, start=start_date, end=end_date)
df_list.append(data)

df = pd.concat(df_list, keys=tickers, names=['Ticker', 'Date'])
print(df.head())
1
import pandas as pd
2
import yfinance as yf
3
from datetime import datetime
4
​
5
start_date = datetime.now() - pd.DateOffset(months=3)
6
end_date = datetime.now()
7
​
8
tickers = ['AAPL', 'MSFT', 'NFLX', 'GOOG']
9
​
10
df_list = []
11
​
12
for ticker in tickers:
13
data = yf.download(ticker, start=start_date, end=end_date)
14
df_list.append(data)
15
​
16
df = pd.concat(df_list, keys=tickers, names=['Ticker', 'Date'])
17
print(df.head())
Open High Low Close Adj Close \
Ticker Date  
AAPL 2023-02-08 153.880005 154.580002 151.169998 151.919998 151.688400  
 2023-02-09 153.779999 154.330002 150.419998 150.869995 150.639999  
 2023-02-10 149.460007 151.339996 149.220001 151.009995 151.009995  
 2023-02-13 150.949997 154.259995 150.919998 153.850006 153.850006  
 2023-02-14 152.119995 153.770004 150.860001 153.199997 153.199997

                     Volume

Ticker Date  
AAPL 2023-02-08 64120100  
 2023-02-09 56007100  
 2023-02-10 57450700  
 2023-02-13 62199000  
 2023-02-14 61707600  
In the above code, we first imported the necessary Python libraries and downloaded the historical stock price data for four companies: Apple, Microsoft, Netflix, and Google, for the last three months.

In this dataset, the Date column is the index column in the DataFrame. We need to reset the index before moving forward:

df = df.reset_index()
print(df.head())
1
df = df.reset_index()
2
print(df.head())
Ticker Date Open High Low Close \
0 AAPL 2023-02-08 153.880005 154.580002 151.169998 151.919998  
1 AAPL 2023-02-09 153.779999 154.330002 150.419998 150.869995  
2 AAPL 2023-02-10 149.460007 151.339996 149.220001 151.009995  
3 AAPL 2023-02-13 150.949997 154.259995 150.919998 153.850006  
4 AAPL 2023-02-14 152.119995 153.770004 150.860001 153.199997

    Adj Close    Volume

0 151.688400 64120100  
1 150.639999 56007100  
2 151.009995 57450700  
3 153.850006 62199000  
4 153.199997 61707600  
Now let’s have a look at the performance in the stock market of all the companies:

import plotly.express as px
fig = px.line(df, x='Date',
y='Close',
color='Ticker',
title="Stock Market Performance for the Last 3 Months")
fig.show()
1
import plotly.express as px
2
fig = px.line(df, x='Date',
3
y='Close',
4
color='Ticker',
5
title="Stock Market Performance for the Last 3 Months")
6
fig.show()
Stock Market Performance for the Last 3 Months
Now let’s look at the faceted area chart, which makes it easy to compare the performance of different companies and identify similarities or differences in their stock price movements:

fig = px.area(df, x='Date', y='Close', color='Ticker',
facet_col='Ticker',
labels={'Date':'Date', 'Close':'Closing Price', 'Ticker':'Company'},
title='Stock Prices for Apple, Microsoft, Netflix, and Google')
fig.show()
1
fig = px.area(df, x='Date', y='Close', color='Ticker',
2
facet_col='Ticker',
3
labels={'Date':'Date', 'Close':'Closing Price', 'Ticker':'Company'},
4
title='Stock Prices for Apple, Microsoft, Netflix, and Google')
5
fig.show()
Stock Prices for Apple, Microsoft, Netflix, and Google
Now let’s analyze moving averages, which provide a useful way to identify trends and patterns in each company’s stock price movements over a period of time:

df['MA10'] = df.groupby('Ticker')['Close'].rolling(window=10).mean().reset_index(0, drop=True)
df['MA20'] = df.groupby('Ticker')['Close'].rolling(window=20).mean().reset_index(0, drop=True)

for ticker, group in df.groupby('Ticker'):
print(f'Moving Averages for {ticker}')
print(group[['MA10', 'MA20']])
1
df['MA10'] = df.groupby('Ticker')['Close'].rolling(window=10).mean().reset_index(0, drop=True)
2
df['MA20'] = df.groupby('Ticker')['Close'].rolling(window=20).mean().reset_index(0, drop=True)
3
​
4
for ticker, group in df.groupby('Ticker'):
5
print(f'Moving Averages for {ticker}')
6
print(group[['MA10', 'MA20']])
Moving Averages for AAPL
MA10 MA20
0 NaN NaN
1 NaN NaN
2 NaN NaN
3 NaN NaN
4 NaN NaN
.. ... ...
56 166.631000 165.2730
57 166.837999 165.3915
58 166.819998 165.4825
59 166.733998 165.5840
60 167.588998 166.0295

[61 rows x 2 columns]
Moving Averages for GOOG
MA10 MA20
183 NaN NaN
184 NaN NaN
185 NaN NaN
186 NaN NaN
187 NaN NaN
.. ... ...
239 106.209000 106.416500
240 106.295000 106.470000
241 106.405001 106.520000
242 106.336001 106.533001
243 106.366500 106.398750

[61 rows x 2 columns]
Moving Averages for MSFT
MA10 MA20
61 NaN NaN
62 NaN NaN
63 NaN NaN
64 NaN NaN
65 NaN NaN
.. ... ...
117 291.889999 289.487000
118 293.594000 290.395999
119 295.188998 291.256999
120 297.119000 292.310500
121 299.607999 293.262999

[61 rows x 2 columns]
Moving Averages for NFLX
MA10 MA20
122 NaN NaN
123 NaN NaN
124 NaN NaN
125 NaN NaN
126 NaN NaN
.. ... ...
178 326.276999 333.262498
179 324.661996 331.725998
180 324.279996 330.353497
181 323.822995 329.274997
182 323.300995 328.446498

[61 rows x 2 columns]
Now here’s how to visualize the moving averages of all companies:

for ticker, group in df.groupby('Ticker'):
fig = px.line(group, x='Date', y=['Close', 'MA10', 'MA20'],
title=f"{ticker} Moving Averages")
fig.show()
1
for ticker, group in df.groupby('Ticker'):
2
fig = px.line(group, x='Date', y=['Close', 'MA10', 'MA20'],
3
title=f"{ticker} Moving Averages")
4
fig.show()
moving averages of apple
moving averages of google
moving averages of Microsoft
moving averages of netflix
The output shows four separate graphs for each company. When the MA10 crosses above the MA20, it is considered a bullish signal indicating that the stock price will continue to rise. Conversely, when the MA10 crosses below the MA20, it is a bearish signal that the stock price will continue falling.

Let us now analyze the volatility of all companies. Volatility is a measure of how much and how often the stock price or market fluctuates over a given period of time. Here’s how to visualize the volatility of all companies:

df['Volatility'] = df.groupby('Ticker')['Close'].pct_change().rolling(window=10).std().reset_index(0, drop=True)
fig = px.line(df, x='Date', y='Volatility',
color='Ticker',
title='Volatility of All Companies')
fig.show()
1
df['Volatility'] = df.groupby('Ticker')['Close'].pct_change().rolling(window=10).std().reset_index(0, drop=True)
2
fig = px.line(df, x='Date', y='Volatility',
3
color='Ticker',
4
title='Volatility of All Companies')
5
fig.show()
stock market performance analysis: volatility
High volatility indicates that the stock or market experiences large and frequent price movements, while low volatility indicates that the market experiences smaller or less frequent price movements.

Now let’s analyze the correlation between the stock prices of Apple and Microsoft:

# create a DataFrame with the stock prices of Apple and Microsoft

apple = df.loc[df['Ticker'] == 'AAPL', ['Date', 'Close']].rename(columns={'Close': 'AAPL'})
microsoft = df.loc[df['Ticker'] == 'MSFT', ['Date', 'Close']].rename(columns={'Close': 'MSFT'})
df_corr = pd.merge(apple, microsoft, on='Date')

# create a scatter plot to visualize the correlation

fig = px.scatter(df_corr, x='AAPL', y='MSFT',
trendline='ols',
title='Correlation between Apple and Microsoft')
fig.show()
1

# create a DataFrame with the stock prices of Apple and Microsoft

2
apple = df.loc[df['Ticker'] == 'AAPL', ['Date', 'Close']].rename(columns={'Close': 'AAPL'})
3
microsoft = df.loc[df['Ticker'] == 'MSFT', ['Date', 'Close']].rename(columns={'Close': 'MSFT'})
4
df_corr = pd.merge(apple, microsoft, on='Date')
5
​
6

# create a scatter plot to visualize the correlation

7
fig = px.scatter(df_corr, x='AAPL', y='MSFT',
8
trendline='ols',
9
title='Correlation between Apple and Microsoft')
10
fig.show()

stock market performance analysis: Correlation between Apple and Microsoft
There is a strong linear relationship between the stock prices of Apple and Microsoft, which means that when the stock price of Apple increases, the stock price of Microsoft also tends to increase. It is a sign of a strong correlation or similarity between the two companies, which can be due to factors such as industry trends, market conditions, or common business partners or customers. For investors, this positive correlation may indicate an opportunity to diversify their portfolio by investing in both companies, as both stocks may offer similar potential returns and risks.
