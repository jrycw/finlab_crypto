<a href="url"><img src="https://i.ibb.co/qWCSkr9/Picture1.png" align="center" width="200px"></a>
[![Build Status](https://travis-ci.com/finlab-python/finlab_crypto.svg?branch=master)](https://travis-ci.com/finlab-python/finlab_crypto) [![PyPI version](https://badge.fury.io/py/finlab-crypto.svg)](https://badge.fury.io/py/finlab-crypto) [![codecov](https://codecov.io/gh/finlab-python/finlab_crypto/branch/master/graph/badge.svg?token=POS648UJ10)](https://codecov.io/gh/finlab-python/finlab_crypto)

Develop and verify crypto trading strategies at a glance.

## Key Features
* Pandas vectorize backtest
* Talib wrapper to composite strategies easily
* Backtest visualization and analysis (uses [vectorbt](https://github.com/polakowo/vectorbt/) as backend)
* Analaysis the probability of overfitting ([combinatorially symmetric cross validation](https://www.davidhbailey.com/dhbpapers/backtest-prob.pdf))
* Easy to deploy strategies on google cloud functions
* Colab and Jupyter compatable
* [10 hours trading bot online course](https://hahow.in/cr/crypto-python)

## Installation
```
pip install finlab_crypto
```

## Colab Example
 * [basic example for backtesting and optimization ![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg) ](https://colab.research.google.com/drive/1l1hylhFY-tzMV1Jca95mv_32hXe0L0M_?usp=sharing)

## Usage
### Setup Research Environment (Recommend)
Create directory `./history/` for saving historical data. If Colab notebook is detected, it creates `GoogleDrive/crypto_workspace/history` and link the folder to `./history/`.
``` python
import finlab_crypto
finlab_crypto.setup()
```
### Get Historical Price
``` python
ohlcv = finlab_crypto.crawler.get_all_binance('BTCUSDT', '4h')
ohlcv.head()
```
!['dataframe'](https://i.ibb.co/YP8Q66m/Screen-Shot-2020-11-23-at-9-33-25-AM.png)
### Trading Strategy
``` python
@finlab_crypto.Strategy(n1=20, n2=60)
def sma_strategy(ohlcv):
  n1 = sma_strategy.n1
  n2 = sma_strategy.n2
  
  sma1 = ohlcv.close.rolling(int(n1)).mean()
  sma2 = ohlcv.close.rolling(int(n2)).mean()
  return (sma1 > sma2), (sma1 < sma2)
```
### Backtest
``` python
# default fee and slipagge are 0.1% and 0.1%

vars =  {'n1': 20, 'n2': 60}
portfolio = sma_strategy.backtest(ohlcv, vars, freq='4h', plot=True)
```
![image](https://media.giphy.com/media/tv4xpwJ3T1zJGV6Smj/giphy.gif)
### Optimization
``` python
import numpy as np
vars = {
  'n1': np.arange(10, 100, 5), 
  'n2': np.arange(10, 100, 5)
}
portfolio = sma_strategy.backtest(ohlcv, vars, freq='4h', plot=True)
```
![cumulative returns](https://i.ibb.co/vxMV4yG/Screen-Shot-2020-11-23-at-9-49-06-AM.png)
![parameter performance](https://i.ibb.co/McrKYDc/Screen-Shot-2020-11-23-at-9-49-15-AM.png)
![parameter range view](https://i.ibb.co/q9d1YHG/Screen-Shot-2020-11-23-at-9-49-28-AM.png)

### Testing

The following script runs all testcases on your local environment. [Creating an isolated python environment](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-with-commands) is recommended. To test crawler functions, please provide Binance API's key and secret by setting environment variables `BINANCE_KEY` and `BINANCE_SECRET`, respectively.

``` bash
git clone https://github.com/finlab-python/finlab_crypto.git
cd finlab_crypto
pip install requirements.txt
pip install coverage
BINANCE_KEY=<<YOUR_BINANCE_KEY>> BINANCE_SECRET=<<YOUR_BINANCE_SECRET>> coverage run -m unittest discover --pattern *_test.py
```

## Updates
Version 0.2.5
* fix weight_btc error
* fix strategy mutable input

Verison 0.2.4
* fix entry price online.py

Version 0.2.3
* fix execution price issue

Version 0.2.2: not stable
* improve syntax
* add execution price for strategy

Version 0.2.1
* fix vectorbt version

Version 0.2.0
* update vectorbt to 0.14.4

Version 0.1.19
* refactor(strategy.py): refactor strategy
* refactor(cscv.py): refactor cscv
* add cscv_nbins and cscv_objective to strategy.backtest
* add bitmex support

Version 0.1.18
* fix(crawler): get_n_bars
* fix(TradingPortfolio): get_ohlcv
* fix(TradingPortfolio): portfolio_backtest

Version 0.1.17
* fix error for latest_signal asset_btc_value
* add unittest for latest_signal

Version 0.1.16
* fix web page error
* fix error for zero orders

Version 0.1.15
* fix web page error

Version 0.1.14
* refine render_html function

Version 0.1.13
* refine display html for TradingPortfolio

Version 0.1.12
* add delay when portfolio backtesting
* fix colab compatability
* improve interface of TradingPortfolio

Version 0.1.11
* fix portfolio backtest error
* add last date equity for backtest

Version 0.1.10
* add portfolio backtest
* rename online.py functions
* refactor error tolerance of different position in online.py functions
* set usdt to excluded asset when calculate position size

Version 0.1.9
* set 'filters' as an optional argument on TradingMethod
* set plot range dynamically
* portfolio backtest

Version 0.1.8
* fix talib parameter type incompatable issue

Version 0.1.7
* fix talib parameter type incompatable issue

Version 0.1.6
* fix talib-binary compatable issue using talib_strategy or talib_filter

Version 0.1.5
* add filters to online.py
* add lambda argument options to talib_filter
* move talib_filter to finlab_crypto package

Version 0.1.4
* fix talib filter and strategy pandas import error
* fix talib import error in indicators, talib_strategy, and talib_filter

Version 0.1.3
* remove progress bar when only single strategy is backtested
* adjust online portfolio to support leaverge
* new theme for overfitting plots
* fix online order with zero order amount
* fix SD2 for overfitting plots

Version 0.1.2
* fix strategy variables

Version 0.1.1
* fix talib error
* add filters folder
* add excluded assets when sync portfolio
* add filter folder to setup
* fix variable eval failure

Version 0.1.0
* add filter interface
* add talib strategy wrapper
* add talib filter wrapper

Version 0.0.9.dev1
* vectorbt heatmap redesign
* improve optimization plots
* redesign strategy interface
* add new function setup, to replace setup_colab

Version 0.0.8.dev1
* fix transaction duplicate bug

Version 0.0.7.dev1
* fix bugs of zero transaction

Version 0.0.6.dev1
* fix latest signal
* rename strategy.recent_signal
* restructure rebalance function in online.py

Version 0.0.5.dev1
* add init module
* add colab setup function
* set vectorbt default
* fix crawler duplicated index

Version 0.0.4.dev1
* add seaborn to dependencies
* remove talib-binary from dependencies
* fix padding style

Version 0.0.3.dev1
* remove logs when calculating portfolio
* add render html to show final portfolio changes
* add button in html to place real trade with google cloud function

Version 0.0.2.dev1
* skip heatmap if it is broken
* add portfolio strategies
* add talib dependency
