---
title: 'Perdagangan Algoritma - Bhg II: Backtest & Alert'
date: 2025-12-15 22:41:50
updated_at: 2025-12-15 22:41:54
tags:
- tutorial
- trading
---

<img alt="backtesting.py" src="https://i.imgur.com/gvFn4Wn.png">

Pergi ke [Bahagian I][2]

Dari dulu lagi bercita-cita nak kasi trading strategy aku di automasi kan. Kali ni dengan bantuan LLM, akhirnya aku berjaya bina stack aku sendiri. Stack yang aku bina ni takde lah advanced sangat kalau nak dibandingkan dengan stack teknologi yang quant dari firm besar pakai, tapi bagi aku dah cukup untuk permulaan seorang trader yang baru nak berkecimpung dalam dunia algo trade.

Kalau dulu aku buat dari scratch, memang berterabur code aku. Tak ada standard, kalau nak ubah parameter kena ubah semua file. Sekarang bila dah tahu konsep sebenar, baru boleh atur dengan jelas. 

Basically nak bina stack backtest ni, langkah pertama kena ada strategy yang kita cipta atau jumpa di internet. Dalam post ni pun aku akan cerita pasal strategy MA crossover biar senang nak cerita. Aku juga ada buat alert untuk di hantar ke Telegram channel. Aku tak host dekat mana-mana cloud server pun, cuma guna Raspberry Pi untuk run script alert ni.

---
**Kandungan:**

- [Permulaan](#permulaan)
  - [Apa itu backtest?](#apa-itu-backtest)
  - [Prosedur operasi](#prosedur-operasi)
  - [Tech stack](#tech-stack)
- [Pembinaan Tapak](#pembinaan-tapak)
  - [Struktur projek](#struktur-projek)
  - [Risk management](#risk-management)
  - [Indicator: Simple Moving Average (SMA)](#indicator-simple-moving-average-sma)
  - [Strategy: Simple MA cross](#strategy-simple-ma-cross)
  - [Historical price data](#historical-price-data)
- [Backtesting](#backtesting)
  - [Bagaimana ia berfungsi](#bagaimana-ia-berfungsi)
  - [Backtesting results](#backtesting-results)
  - [Optimization](#optimization)
- [Alert Bot](#alert-bot)
  - [Live Alerts](#live-alerts)
- [Control From CLI](#control-from-cli)
  - [Backtesting](#backtesting-1)
  - [Alert bot](#alert-bot-1)
- [Deployment Ke Raspberry Pi](#deployment-ke-raspberry-pi)
  - [Common Linux commands](#common-linux-commands)
- [Trade-Offs](#trade-offs)
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

Kenapa TradingView?

Mungkin lain orang ada cara nya tersendiri. Tapi sebagai seorang yang cheapskate macam aku, aku akan mulakan dengan benda yang free dulu. TradingView ada juga feature untuk backtest strategi menggunakan Pine Script (scripting language untuk TradingView). Cuma historical price data untuk free tier nya terhad kepada 5,000 bars sahaja (intraday timeframe). Jadi tak syok la tak dapat guna semua data yang ada. Macam aku cakap tadi, lagi banyak data kita ada, lagi bagus result backtest.

> ⚠⚠ Iklan ⚠⚠  
> Register akaun TradingView menggunakan ref link ini:  
> [https://www.tradingview.com/pricing/?share_your_love=natebroke][3]  
> Kita kan kawan :)

### Tech stack

Tech yang digunakan sepanjang perjalanan backtesting ni adalah:

|||
|---|---|
| **Language** | Python, Pine Script |
| **Backtest framework** | [backtesting.py][1] |
| **Alert** | Telegram |
| **LLM** | Grok, ChatGPT |
| **Server** | Raspberry Pi 3 Model B Rev 1.2, 1GB |
| **Server OS** | Debian 12 (bookworm) |

Tak ada Raspberry Pi pun takpe, benda tu untuk kita jalankan alert bot selama 24/7. Guna laptop pun boleh, cuma kena biarkan on je lah.

Untuk engine backtest, aku gunakan [backtesting.py][1] framework. Ini adalah framework untuk backtest strategy trading yang ditulis dalam Python.

Aku pilih framework ini sebab aku tengok dia *macam* lightweight, sebab feature nya tak sebanyak framework backtest lain. Mula-mula aku nak guna [Hummingbot][4], tapi learning curve dia agak curam ya. Walaupun guna LLM, aku tetap jadi bodoh dalam mengintegrasikan Hummingbot dalam sistem aku. Mungkin sebab sasaran objektif aku cuma sikit je kot; nak dapatkan backtest result. Bila dah ada banyak feature yang bloated dalam sesebuah framework tu kan otak kita jadi overwhelm.

Jadi backtesting.py ni engine nya hanya fokus kepada kerja nak backtest saja. Tak ada pun feature untuk live trade mahupun native Telegram alert. Ya, untuk alert tu aku custom sendiri. Bahagian Alert nanti aku ada cerita.

## Pembinaan Tapak
### Struktur projek

I like things tidy. Here's how I set it up:

```bash
ma-cross-bot/
├── config/
│   ├── exchange.py      # symbol, interval
│   └── strategy.py      # MA lengths
│   └── trading.py       # risk management
│── indicators/
│   └── ma.py            # indicator code
├── core/
│   └── ma_cross.py      # the Strategy class
├── utils/
│   ├── logger.py        # file logging
│   ├── binance_client.py # smart data fetch
│   ├── live_alert.py     # common signal alert code
│   └── backtest_runner.py # common backtest code
├── data/cache/          # saved CSVs
├── logs/                # log files
├── backtest.py          # run backtests
└── live_alert.py        # live signal check
```

Configs separate, strategy isolated, utils reusable.

### Risk management

TODO trading.py, sl, tp, trailing

### Indicator: Simple Moving Average (SMA)

TODO config.py
TODO sma.py

### Strategy: Simple MA cross

<img alt="ma-cross-tv" src="https://i.imgur.com/JKj6MET.png">

Script di bawah adalah strategy untuk MA crossing. Dimana kondisi untuk melakukan entry dan exit adalah seperti berikut:

1. `fast_ma` cross atas `slow_ma`: signal beli 🟢
2. `fast_ma` cross bawah `slow_ma`: signal jual 🔴

Simple je, demi contoh.

**Pine Script:**

```javascript
fast_ma = sma(price, fast_length)
slow_ma = sma(price, slow_length)

longCondition = crossover(fast_ma, slow_ma)
if (longCondition)
    strategy.entry("Long", strategy.long)

shortCondition = crossunder(fast_ma, slow_ma)
if (shortCondition)
    strategy.entry("Short", strategy.short)
```

Nak baca Pine Script ni straightforward je. Tetapkan nilai `fast_length` dan `slow_length`, lepas tu compute untuk `crossover` atau `crossunder`.

Berikut adalah code yang di terjemahkan ke dalam Python:

*Nota: Nak senang, guna LLM seperti Grok atau ChatGPT untuk convert Pine Script ke Python. Kalau ada bug tu pandai-pandai lah betulkan.*

**Python:**

```python
class MaCross(Strategy):
    fast_length = FAST_LENGTH
    slow_length = SLOW_LENGTH

    def init(self):
        self.fast_ma = self.I(SMA, self.data.Close, self.fast_length)
        self.slow_ma = self.I(SMA, self.data.Close, self.slow_length)

    def next(self):
        if crossover(self.fast_ma, self.slow_ma):
            self.buy()
        elif crossover(self.slow_ma, self.fast_ma):
            self.sell()
```

`class MaCross(Strategy)`:

Kita extend class `MaCross()` dengan `Strategy` base class dari package backtesting.py, untuk menyatakan yang class ini bersedia untuk di consume sebagai strategy. Kita juga perlu override `init()` dan `next()` methods untuk kita define strategy kita.

`def init()`:

Letak segala initialization seperti settings untuk indicator ke dalam function ini.

`self.I()`:

Function ini untuk kita declare indicator seperti SMA, RSI, MACD dan lain-lain. Dalam contoh ini, kita menggunakan SMA; ia memerlukan dua parameter iaitu *source* dan *length*. Jadi kita perlu pass kan parameter itu menggunakan function `I()` ini. Class untuk indicator SMA nanti kita akan buat.

`def next()`:

Bahagian ini untuk compute keputusan strategi, samada perlu buy atau sell. Logic seperti `crossover` atau `crossunder` diletakkan disini. Sama seperti Pine Script tadi, signal buy atau sell akan berlaku jika kondisi berlaku seperti apa yang kita nak. Di dalam function ini juga info tentang position dah enter atau dah close boleh dimainkan. Info penuh tentang candlestick (OHLCV) juga dikeluarkan di dalam function ini. Jadi kalau nak modify apa-apa pasal position (`is_long`, `is_short`, etc.), boleh buat dalam ni.

### Historical price data

TODO code

Untuk historical price data, dalam projek ni aku cuma ambil dari Binance spot market. Tak pasti boleh tarik dari perpetual market ke tak, pasal belum cuba. Jadi aku ada script untuk bot ni auto download price data untuk mana-mana pair dalam Binance, dari berbagai timeframe yang ada.

Script ni basically akan check cache folder dulu, kalau dah ada data yang pernah di download sebelum ni, cuma perlu append yang candle yang baru je lah. Aku juga ada buat option untuk *force download* data historical ni, biar clean sikit kerja. So, first run akan jadi perlahan, bila semua data dah di download dan disimpan, run kedua akan jadi laju.

<img alt="data-download-binance" src="https://i.imgur.com/80yLt6R.png">

## Backtesting

### Bagaimana ia berfungsi

Jadi kita ada `core/strategy.py`, class ini inherit object `Strategy` yang akan di consume oleh framework.

`config.py` dicantumkan dengan `strategy.py` dan ia disalurkan ke backtest atau alert runner.

The backtest script loads config, fetches data, runs the backtest, shows plot. Add `--optimize` for parameter search.

TODO fractional, behavior (trade on close, finalize trade, exclusive trade)

<img alt="backtest-runner-flow" src="https://i.imgur.com/7aCja4i.png">

### Backtesting results

Trading settings:

> Symbol: BTCUSDT  
> Exchange: Binance  
> Timeframe: 4H

Strategy settings:

> MA Type: SMA  
> MA Source: Close  
> Fast MA: 14  
> Slow MA: 114

Hasil backtest dari TradingView:

<img alt="sma cross btcusdt:binance 4h" src="https://i.imgur.com/GvyxPrF.png">

*Nota: Historical data terhad sebab akaun aku free tier :^)*

Hasil backtest dari backtesting.py:

TODO

### Optimization

## Alert Bot

Pasal alert bot ni aku nak cerita sedikit tentang sejarah. Dulu aku pernah beli TradingView premium. Yang akaun premium ni biasa lah macam-macam feature extra kita dapat. Yang paling aku pakai waktu tu adalah *webhook alert* bila signal dikeluarkan oleh strategy. Benda ni senang gila nak pakai. Lepas kita settle dan puas hati dengan result backtest, kita boleh terus hook alert ni ke mana-mana API.

Waktu tu aku pakai platform [Wunderbit Trading][5]. Tempat ni kita boleh automate kan trading, kira macam algo trade la juga cuma tak perlu pun tulis code bagai dan tak perlu deposit duit. Cukup ada TradingView + webhook alert, lepas tu connect dengan account CEX dan connect dengan Wunderbit ni, terus dia boleh auto-trade mengikut signal strategy dari TradingView tadi.

Sekarang aku dah tak pakai benda tu sebab free trial dah habis. Maka nya di sini lah aku, berjalan keseorangan di atas gurun sahara membina sendiri trading stack dari scratch.

### Live Alerts

TODO masih gunakan backtest runner untuk berfungsi

Basically kita intercept data result dari bt.run() untuk kenalpasti signal baru, lepas tu baru hantar ke telegram.

Set env telegram channel token dan id.

For live, the script runs the same backtest on every new closed candle, checks if a trade would enter on the last bar, sends Telegram if yes.

I run it with systemd timer—every 5 minutes for 5m chart. Survives reboots, network drops.

<img alt="alert-bot-flow" src="https://i.imgur.com/7dVnkUV.png">

## Control From CLI

Refer to `utils/args.py`

### Backtesting

```bash
python backtest_ma.py --symbol BTCUSDT --interval 1h --start 2025-05-01 --fractional --no-plot
```

### Alert bot

```bash
python alert_ma.py --symbol BTCUSDT --interval 1h --test-alert
```

## Deployment Ke Raspberry Pi

Copy the folder over, set up venv, install requirements.

Set Telegram env vars.

Create systemd service and timer.

Enable it.

Done. Runs quietly, logs to file, alerts on phone.

### Common Linux commands

Ini SOP aku, bila mana aku nak mulakan alert untuk strategy lain atau nak create timer systemd baru sehinggalah untuk lihat log report untuk strategy yang dijalankan.

## Trade-Offs

Framework khas untuk backtesting saja. Live alert sebenarnya terlewat satu candle, tak sama dengan behavior tradingview. alert juga untuk entry/reversal entry dan bukan close position signal sebab backtesting.py tak salurkan info ni. berhari2 aku cuba hack untuk dapatkan info close trade, tapi tak dapat. kalau set finalize_trade=true, dia akan close setiap candle baru. kalau false, dia tak plot pun position yang paling latest yang masih open. closed_trades property yang null tu untuk closed trade yang dah lepas, bukan yang latest.

jadi nak yang accurate kena pakai framework khas untuk live trading, lain kata, framework yang calculate trade on tick.

## Penutup

backtesting.py made this possible without much hassle. The code stays the same for backtest and live. No drift.

It's basic, but reliable. When I want to add filters or more strategies, the structure is ready.

That's how I got a simple MA cross strategy from idea to running on a Pi. Clean, organized, and useful.

[1]: https://kernc.github.io/backtesting.py/
[2]: https://luangdiri.github.io/2021/08/21/perdagangan-algoritma-bhg-1.html
[3]: https://www.tradingview.com/pricing/?share_your_love=natebroke
[4]: https://hummingbot.org/
[5]: https://wundertrading.com/en/register?ref=wbtdf816e60