---
title: "Perdagangan Algoritma"
date: 2021-08-21 21:28:55
lastmod: 2022-12-07 23:55:15
---

Draft:

1. Cari strat
2. Berlatih sebulan guna cara manual
   1. penting utk faham strat behavior
3. Apa itu strat behavior?
   1. lain pair, lain behavior bila kita guna algo
4. Scripting
   1. Convert ke algorithm
   2. Convert ke pinescript/python
5. Backtest di Tradingview/python lib
   1. Guna alert utk tengok keberkesanan
      1. sebab past backtest =/= future
      2. penting untk faham strat behavior guna past data
6. Live trade
   1. python/js/c
      1. ccxt, etc
      2. diy coding
   2. basic code structure
      1. get historical data
      2. stream candle
      3. while checking algorithm
      4. execute trades if algo checks
      5. logging

**Backtest**
- tradingview
- python diy

**Bahasa skrip**
- pine script
- python
- js
- c

```
import lib

global var
local var

create_order():

handle_sell():

handle_buy():

get_asset():

get_ust():

calc_pnl():

broadcast():

calc_strategy():
   

candle_data():

   # websocket should give current candle data here

   if is_candle_closed:
      signal = calc_strategy(last_price)
      if signal is buy:
         create_order(side=buy)
      elif signal is sell:
         create_order(side=sell)
connect_websocket():

main():
```

F:\Workspace\Binance-rsi-bot

see @r5d2_trades for info
https://t.me/intredasting/1952


- strategy tips https://www.thechartist.com.au/strategies-that-will-continue-to-profit/
- monte carlo

#list algo trade

https://github.com/ccxt/

https://algotrading101.com/learn/binance-python-api-guide/

https://www.hodlbot.io/blog/ultimate-guide-on-crypto-trading-bots

https://medium.com/@LeonFedden/deep-cryptocurrency-trading-1e64af6d280a

https://medium.com/@joeldg/an-advanced-tutorial-a-new-crypto-currency-trading-bot-boilerplate-framework-e777733607ae

https://www.freqtrade.io/en/latest/

https://xtcryptosignals.com/

https://github.com/btrccts/btrccts/

https://algotrading101.com/learn/pine-script-tradingview-guide/

https://algotrading101.com/learn/yfinance-guide/

https://github.com/bobthebot101/Binance-rsi-bot

Trading App Tutorial — https://www.youtube.com/watch?v=OFnOEIyqSRg&list=PLvzuUVysUFOuoRna8KhschkVVUo2E2g6G

https://github.com/MaddieLoki/BinanceAPITutorial

Plotting with TA-Lib example — https://blog.quantinsti.com/install-ta-lib-python/

Historical data in csv — https://www.cryptodatadownload.com/data/binance/

Retreieve historical data — https://medium.com/swlh/retrieving-full-historical-data-for-every-cryptocurrency-on-binance-bitmex-using-the-python-apis-27b47fd8137f

Multiple asset stream — https://stackoverflow.com/questions/53997367/how-can-i-open-multiple-websocket-streams

Backtesting — https://medium.com/coinmonks/back-testing-and-deploying-a-automated-trading-strategy-to-ftx-f84073ee1ad6

How is backtesting accurate?
- tradingview backtest is not accurate. if you put trailing stop in the script, it would give an inaccurate result (source (https://backtest-rookies.com/2018/06/08/tradingview-trailing-stop-mechanics-beware-version-3/))
- it's never accurate. just lower the drawdown and more to profit/exit.

Backtest python backtrader — https://github.com/mementum/backtrader
https://www.youtube.com/watch?v=B6I-Fd2z6Gk
https://www.youtube.com/watch?v=UNkH1TQl7qo&list=PLvzuUVysUFOs2kZB5KbsAPi0XgqemA5s9

RSI backtest — https://github.com/SharmaVidhiHaresh/Backtesting-Trading-Strategies-with-Python

Bactesting lib — https://kernc.github.io/backtesting.py/

https://github.com/Superalgos/Superalgos
https://superalgos.org/index.shtml

https://github.com/vsnz/TradingView-Webhook-Bot

Trading Terminal — https://margin.de/

https://www.quantower.com/pricing

Strategies that works — https://www.thechartist.com.au/strategies-that-will-continue-to-profit/

repainting

https://medium.com/coinmonks/how-to-get-historical-crypto-currency-data-954062d40d2d

https://github.com/bitfinexcom/bitfinex-api-py