# Mathematical_Trading226
import yfinance as yf
import pandas as pd
import numpy as np
from scipy.stats import norm

indices = ['^GSPC', '^HSI', '^GDAXI', '^DJI', '^N225']
equities = ['SONY', 'MSFT', 'TM', 'GOOGL', 'SSNLF']

start_date = '2010-01-01'
end_date = pd.Timestamp.today().strftime('%Y-%m-%d')
data = yf.download(indices + equities, start=start_date, end=end_date)['Adj Close']

returns = data.pct_change()

cumulative_returns = (1 + returns).cumprod()

rolling_max = cumulative_returns.rolling(window=len(cumulative_returns), min_periods=1).max()
drawdown = cumulative_returns / rolling_max - 1
max_drawdowns = drawdown.min()

daily_returns_mean = returns.mean()
daily_returns_std = returns.std()

risk_free_rate = 0.0

sharpe_ratio = (daily_returns_mean - risk_free_rate) / daily_returns_std

negative_returns = returns[returns < 0]
downside_returns_std = negative_returns.std()
sortino_ratio = (daily_returns_mean - risk_free_rate) / downside_returns_std

print("Daily Returns:")
print(returns.tail())

print("\nCumulative Returns:")
print(cumulative_returns.tail())

print("\nMax Drawdowns:")
print(max_drawdowns)

print("\nSharpe Ratio:")
print(sharpe_ratio)

print("\nSortino Ratio:")
print(sortino_ratio)
