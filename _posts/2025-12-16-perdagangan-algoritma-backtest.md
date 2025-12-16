---
title: 'Perdagangan Algoritma - Bhg II: Back Test & Alert'
date: 2025-12-16 22:41:50
updated_at: 2025-12-16 22:41:54
tags:
- tutorial
- trading
---

*Draft written by Grok*

Finally, I managed to get a clean setup for testing and alerting on a basic moving average crossover strategy. Before this, everything was scattered—quick tests in notebooks, alerts hacked together in random scripts. It worked, but it was messy. Too many places to change parameters, data all over the place.

I remember when I first started with backtesting.py. It was straightforward—no heavy setup, just a class and a DataFrame. But over time, as I added more features like RSI filters and higher-timeframe checks, things got complicated. I needed something organized.

In the end, I settled on a modular structure. All configs in one place, core strategy separate, utils for shared stuff. And for live alerts, a simple script that runs the same backtest on fresh data.

This is the journey of putting it all together, from folder setup to running on a Raspberry Pi.

## The Strategy: Simple MA Cross

Nothing fancy. Fast moving average crosses above slow for long, below for short. That's it. No filters this time—just to keep things simple and fast.

The core looks like this:

```python
class MaCross(Strategy):
    fast_length = FAST_LENGTH
    slow_length = SLOW_LENGTH

    def init(self):
        self.fast = self.I(SMA, self.data.Close, self.fast_length)
        self.slow = self.I(SMA, self.data.Close, self.slow_length)

    def next(self):
        if crossover(self.fast, self.slow):
            self.buy()
        elif crossover(self.slow, self.fast):
            self.sell()
```

Easy to read, easy to change.

## Project Structure

I like things tidy. Here's how I set it up:

```
ma-cross-bot/
├── config/
│   ├── exchange.py      # symbol, interval
│   └── strategy.py      # MA lengths
├── core/
│   └── ma_cross.py      # the Strategy class
├── utils/
│   ├── logger.py        # file logging
│   ├── binance_client.py # smart data fetch
│   └── backtest_runner.py # common backtest code
├── data/cache/          # saved CSVs
├── logs/                # log files
├── backtest.py          # run backtests
└── live_alert.py        # live signal check
```

Configs separate, strategy isolated, utils reusable.

## Data from Binance

Data comes from Binance. The downloader checks cache first, appends only new candles. No full redownload every time.

First run takes a while, but after that it's quick.

## Backtesting

The backtest script loads config, fetches data, runs the backtest, shows plot. Add --optimize for parameter search.

Simple.

## Live Alerts

For live, the script runs the same backtest on every new closed candle, checks if a trade would enter on the last bar, sends Telegram if yes.

I run it with systemd timer—every 5 minutes for 5m chart. Survives reboots, network drops.

## Deployment on Raspberry Pi

Copy the folder over, set up venv, install requirements.

Set Telegram env vars.

Create systemd service and timer.

Enable it.

Done. Runs quietly, logs to file, alerts on phone.

## Final Thoughts

backtesting.py made this possible without much hassle. The code stays the same for backtest and live. No drift.

It's basic, but reliable. When I want to add filters or more strategies, the structure is ready.

That's how I got a simple MA cross strategy from idea to running on a Pi. Clean, organized, and useful.