---
title: 'Perdagangan Algoritma - Bhg II: Backtest & Alert'
date: 2025-12-15 22:41:50
updated_at: 2025-12-15 22:41:54
tags:
- tutorial
- trading
---

<img alt="backtesting.py" src="https://i.imgur.com/gvFn4Wn.png">

Dari dulu lagi bercita-cita nak kasi trading strategy aku di automasi kan. Kali ni dengan bantuan LLM, akhirnya aku berjaya bina stack aku sendiri. Stack yang aku bina ni takde lah advanced sangat kalau nak dibandingkan dengan stack teknologi yang quant dari firm besar pakai, tapi bagi aku dah cukup untuk permulaan seorang trader yang baru nak berkecimpung dalam dunia algo trade.

Kalau dulu aku buat dari scratch, memang berterabur code aku. Tak ada standard, kalau nak ubah parameter kena ubah semua file. Sekarang bila dah tahu konsep sebenar, baru boleh atur dengan jelas. 

Basically nak bina stack backtest ni, langkah pertama kena ada strategy yang kita cipta atau jumpa di internet. Dalam post ni pun aku akan cerita pasal strategy MA crossover biar senang nak cerita. Aku juga ada buat alert untuk di hantar ke Telegram channel. Aku tak host dekat mana-mana cloud server pun, cuma guna Raspberry Pi untuk run script alert ni.

---
**Kandungan:**

- [Permulaan](#permulaan)
  - [Apa itu backtest?](#apa-itu-backtest)
  - [Prosedur operasi](#prosedur-operasi)
  - [Tech stack](#tech-stack)
- [Pembinaan: Tapak](#pembinaan-tapak)
  - [Struktur projek](#struktur-projek)
  - [Strategi: Simple MA cross](#strategi-simple-ma-cross)
  - [Data from Binance](#data-from-binance)
- [Pembinaan: Backtesting](#pembinaan-backtesting)
  - [Backtesting](#backtesting)
- [Pembinaan: Live Alerts](#pembinaan-live-alerts)
  - [Live Alerts](#live-alerts)
- [Pembinaan: Deployment](#pembinaan-deployment)
  - [Deployment on Raspberry Pi](#deployment-on-raspberry-pi)
- [Penutup](#penutup)

---

## Permulaan
### Apa itu backtest?

Berbalik kepada cerita asal, backtesting adalah satu fasa dalam sistematik trading iaitu menguji strategi dengan menggunakan data harga di masa lampau. Kita nak uji strategi tersebut berkesan atau tidak bergantung kepada keadaan market. Market price ada beberapa fasa juga iaitu, uptrend, sideway dan downtrend. Ada juga orang sebut fasa ini sebagai accumulation, expansion dan distribution. Ikut la mana suka, konteks nya tetap sama.

Bila kita uji strategi kita tu, kita cuma melihat performance nya dengan waktu yang dah berlalu. Bererti ujian tu mungkin boleh atau tak boleh diterapkan untuk perjalanan harga di masa depan. Kata lain, past performance tak menjamin future performance. Itu sebab kita kena backtest strategi tersebut dengan data sample yang banyak.

Data sample dalam hal ini merujuk kepada data sejarah harga dari awal sesebuah pair itu hidup di market sehingga lah sekarang. Lagi banyak data sample yang kita dapat, lebih bermutu final result yang di keluarkan dari ujian backtest.

### Prosedur operasi

Untuk permulaan, kita perlu ada strategi untuk melaksanakan proses sistematik trading ni.

1. Idea strategy
2. Convert strategy jadi code
3. Integrate code dalam framework
4. Lakukan backtest
5. Run alert bot (optional)

Biasa nya prosedur yang biasa aku buat bila dah dapat sesuatu strategi adalah:

1. Backtest dulu dalam TradingView -- guna Pine Script
2. Convert Pine Script ke Python -- guna LLM
3. Integrate ke dalam backtesting.py -- backtest framework

Kenapa TradingView? Mungkin lain orang, lain cara nya. Tapi sebagai seorang yang cheapskate macam aku, aku akan mulakan dengan benda yang free dulu. TradingView ada juga feature untuk backtest strategi menggunakan Pine Script (scripting language untuk TradingView). Cuma historical price data untuk free tier ni terhad kepada 5000 candles sahaja untuk intraday timeframe. Jadi tak syok la tak dapat guna semua data yang ada. Macam aku cakap tadi, lagi banyak data kita ada, lagi bagus result backtest.

> ⚠⚠ Iklan ⚠⚠  
> Register akaun TradingView menggunakan ref link ini: https://www.tradingview.com/pricing/?share_your_love=natebroke  
> Kita kan kawan :)

### Tech stack

Tech yang digunakan dalam perjalanan backtesting ni adalah:

|  |  |
|---|---|
| Language | Python, Pine Script |
| Framework | [backtesting.py][1] |
| Alert | Telegram |
| LLM | Grok, ChatGPT |
| Server | Raspberry Pi 3 Model B Rev 1.2, 1GB |
| Server OS | Debian 12 (bookworm) |

Tak ada Raspberry Pi pun takpe, itu untuk kita jalankan alert 24/7. Guna laptop pun boleh, cuma kena biarkan on je lah.

## Pembinaan: Tapak
### Struktur projek

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

### Strategi: Simple MA cross

<img width="70%" alt="strategy-funnel" src="https://i.imgur.com/eJJutvf.jpeg">

Nothing fancy. Fast moving average crosses above slow for long, below for short. That's it. Boleh juga tambah indicator lain

The core looks like this:

Pine Script:

```
```

Python:

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

### Data from Binance

Data comes from Binance. The downloader checks cache first, appends only new candles. No full redownload every time.

First run takes a while, but after that it's quick.

## Pembinaan: Backtesting
### Backtesting

The backtest script loads config, fetches data, runs the backtest, shows plot. Add --optimize for parameter search.

Simple.

## Pembinaan: Live Alerts
### Live Alerts

For live, the script runs the same backtest on every new closed candle, checks if a trade would enter on the last bar, sends Telegram if yes.

I run it with systemd timer—every 5 minutes for 5m chart. Survives reboots, network drops.

## Pembinaan: Deployment
### Deployment on Raspberry Pi

Copy the folder over, set up venv, install requirements.

Set Telegram env vars.

Create systemd service and timer.

Enable it.

Done. Runs quietly, logs to file, alerts on phone.

## Penutup

backtesting.py made this possible without much hassle. The code stays the same for backtest and live. No drift.

It's basic, but reliable. When I want to add filters or more strategies, the structure is ready.

That's how I got a simple MA cross strategy from idea to running on a Pi. Clean, organized, and useful.

[1]: https://kernc.github.io/backtesting.py/