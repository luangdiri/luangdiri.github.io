---
title: 'Perdagangan Algoritma - Bhg II: Backtest & Alert'
date: 2025-12-15 22:41:50
updated_at: 2025-12-15 22:41:54
tags:
- tutorial
- trading
---

*Work In Progress*

TODO

```
Tips finding strategy
- yg tak repaint

Add full code link

Python package

Source code zip

Boilerplate code utk risk mgt, long short buy sell close stop trailing. Extra like pyramiding partial buy sell

Explain backtest result. Sharpe ratio etc

Bullet point add: logging
```

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
  - [Indicator: Simple MA (SMA)](#indicator-simple-ma-sma)
  - [Strategy: Simple MA cross](#strategy-simple-ma-cross)
  - [Historical price data](#historical-price-data)
- [Backtesting](#backtesting)
  - [Bagaimana ia berfungsi](#bagaimana-ia-berfungsi)
  - [Backtesting results](#backtesting-results)
  - [Parameter optimization](#parameter-optimization)
- [Alert Bot](#alert-bot)
  - [Live Alerts](#live-alerts)
- [Control From CLI](#control-from-cli)
  - [Backtesting Mode](#backtesting-mode)
  - [Live Alert Mode](#live-alert-mode)
  - [Usage Examples](#usage-examples)
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
│   ├── ma_cross.py      # strategy settings
│   └── risk.py       # risk management
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
├── backtest_ma_cross.py          # run backtests
└── alert_ma_cross.py        # live signal check
```

Configs separate, strategy isolated, utils reusable.

### Indicator: Simple MA (SMA)


```python
def sma(series: pd.Series, length: int) -> pd.Series:
    return series.rolling(window=length).mean()
```

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

*Full code: https://gist.github.com/luangdiri/efb937e67f88e3a9ab738c5bd82535f4*

Nak baca Pine Script ni straightforward je. Tetapkan nilai `fast_length` dan `slow_length`, lepas tu compute untuk `crossover` atau `crossunder`.

Berikut adalah code yang di terjemahkan ke dalam Python:

*Nota: Nak senang, guna LLM seperti Grok atau ChatGPT untuk convert Pine Script ke Python. Kalau ada bug tu pandai-pandai lah betulkan.*

**Python:**

```python
class MACrossStrategy(Strategy):
    def init(self):
        close = pd.Series(self.data.Close)
        self.fast_ma = self.I(sma, close, FAST_MA_LENGTH)
        self.slow_ma = self.I(sma, close, SLOW_MA_LENGTH)

    def next(self):
        if crossover(self.fast_ma, self.slow_ma):
            self.position.close()
            self.buy()
        elif crossover(self.slow_ma, self.fast_ma):
            self.position.close()
            self.sell()
```

`class MaCross(Strategy)`:

Kita extend class `MaCross()` dengan `Strategy` base class dari package backtesting.py, untuk menyatakan yang class ini bersedia untuk di consume sebagai strategy. Kita juga perlu override `init()` dan `next()` methods untuk kita define strategy kita.

`def init()`:

Letak segala initialization seperti settings untuk indicator ke dalam function ini.

`self.I()`:

Function ini untuk kita declare indicator seperti SMA, RSI, MACD dan lain-lain. Dalam contoh ini, kita menggunakan SMA; ia memerlukan dua parameter iaitu *source* dan *length*. Jadi kita perlu pass kan parameter itu menggunakan function `I()` ini. Class untuk indicator SMA nanti kita akan buat.

`def next()`:

Bahagian ini untuk compute keputusan strategi, samada perlu buy atau sell. Logic seperti `crossover` atau `crossunder` diletakkan disini. Sama seperti Pine Script tadi, signal buy atau sell akan berlaku jika kondisi berlaku seperti apa yang kita nak. Di dalam function ini juga info tentang position dah enter atau dah close boleh dimainkan. Info penuh tentang candlestick (OHLCV) juga dikeluarkan di dalam function ini. Jadi kalau nak modify apa-apa pasal position (`is_long`, `is_short`, etc.), boleh buat dalam ni. Baca [documentation][7] nya kalau nak tahu lebih lanjut.

**Config:**

```python
STRATEGY_NAME = "MA Cross Strategy"
SHORT_NAME = "ma_cross"
FAST_MA_LENGTH = 25
FAST_MA_TYPE = "SMA"
SLOW_MA_LENGTH = 50
SLOW_MA_TYPE = "SMA"
```

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

**TradingView:**

<img alt="sma_cross_btcusdt_4h" src="https://i.imgur.com/GvyxPrF.png">

*Nota: Historical data terhad sebab akaun aku free tier :^)*

Nampak cantik kan hasil backtest nya? Masalah free tier ni data backtest nya terhad. Cuba tengok yang dari backtesting.py di bawah.

**backtesting.py:**

<img alt="sma_cross_btcusdt_4h_backtesting.py" src="https://i.imgur.com/rhqtB4C.png">

*[Besarkan][6]*

Setelah backtest dijalankan, info statistik berkenaan strategi yang kita backtest tadi akan dikeluarkan dalam terminal:

<img alt="backtesting_result" src="https://i.imgur.com/CNKJRLF.png">

Disini kita boleh tengok performance strategy. Contoh di atas aku backtest dari Januari 2021 sehingga ke 18/12/2025 (waktu aku menulis ni). Hampir 5 tahun punya data. Nak dibandingkan dengan TradingView tadi, jauh beza walau nampak. Banyak info yang dapat ni, aku cuma tengok 1 benda je biasanya: **Sharpe ratio**. Nilai sharpe ratio ni kalau > 1 kira dah power lah. Lain tu aku jarang tengok, tapi berguna juga. Contoh macam nak tahu tempoh drawdown berapa lama, drawdown paling teruk dan equity final.

### Parameter optimization

Bila kita run backtest dalam mode biasa, kita cuma pakai setting indicator dari config file je. Dalam mode optimization, backtest engine akan run multiple setting yang kita set. 

```python
class MACrossStrategy(Strategy):
    fast_length = FAST_MA_LENGTH
    slow_length = SLOW_MA_LENGTH
```

```python
optimize_params = dict(
    fast_ma_length=range(20, 50, 10),
    slow_ma_length=range(50, 200, 20),
)
```

## Alert Bot

Pasal alert bot ni aku nak cerita sedikit tentang sejarah. Dulu aku pernah beli TradingView premium. Yang akaun premium ni biasa lah macam-macam feature extra kita dapat. Yang paling aku pakai waktu tu adalah *webhook alert* bila signal dikeluarkan oleh strategy. Benda ni senang gila nak pakai. Lepas kita settle dan puas hati dengan result backtest, kita boleh terus hook alert ni ke mana-mana API.

Waktu tu aku pakai platform [Wunderbit Trading][5]. Tempat ni kita boleh automate kan trading, kira macam algo trade la juga cuma tak perlu pun tulis code bagai dan tak perlu deposit duit. Cukup ada TradingView + webhook alert, lepas tu connect dengan account CEX dan connect dengan Wunderbit ni, terus dia boleh auto-trade mengikut signal strategy dari TradingView tadi.

Sekarang aku dah tak pakai benda tu sebab free trial dah habis. Maka nya di sini lah aku, berjalan keseorangan di atas gurun sahara membina sendiri trading stack dari scratch.

### Live Alerts

Untuk alert bot, ia masih menggunakan `backtest_runner.py`. Basically kita intercept data result dari `bt.run()` untuk kenalpasti signal baru, kalau ada signal, baru hantar ke Telegram (token dan channel ID di set dalam `.env`).

```python
    stats = bt.run()
    latest_trade = stats._trades.iloc[-1] # check latest trade

    # signal entry long atau short berlaku di bawah
    if latest_trade.EntryBar == last_signal_bar_index:
        return
    if latest_trade.EntryBar != len(df) - 1:
        return

    last_signal_bar_index = latest_trade.EntryBar
    msg = format_signal_alert(latest_trade, msg_details)
    send_telegram(msg)
```

Berikut adalah flow chart proses alert bot:

<img alt="alert-bot-flow" src="https://i.imgur.com/7dVnkUV.png">

Script ini dijalankan sekali sahaja. Untuk buat dia run secara berkala (mengikut timeframe pilihan), guna cronjob atau systemd timer. Aku ada cerita di bawah.

## Control From CLI

Refer to `utils/args.py`

### Backtesting Mode

Command: `python backtest_ma_cross.py [options]` (or equivalent entry point)

**Arguments:**

`--symbol` (required)  
Trading pair symbol, e.g., BTCUSDT

`--interval` (required)  
Candle interval, e.g., 5m, 30m, 1h, 4h, 1d

`--start`  
Start date for historical data (format: YYYY-MM-DD).  
Default: 2022-01-01

`--end`  
End date for historical data (format: YYYY-MM-DD).  
Default: today (current date)

`-f, --force-download`  
Force re-download of data even if cached data exists

`--no-plot`  
Disable plotting of backtest results

`-d, --debug`  
Enable debug mode with verbose logging

`--plot-resample`  
Resample data before plotting to reduce file size and improve performance

`--fractional`  
Enable fractional position sizing in trading simulation

`-o, --optimize`  
Run parameter optimization instead of single backtest

### Live Alert Mode

Command: `python alert_ma_cross.py [options]` (or equivalent entry point)

**Arguments:**

`--symbol (required)`  
Trading pair symbol, e.g., BTCUSDT

`--interval (required)`  
Candle interval, e.g., 5m, 30m, 1h, 4h, 1d

`--test-alert`  
Send a test Telegram alert message on startup

`-v, --verbose`  
Enable verbose logging output

### Usage Examples

Backtesting:  

`python run_backtest.py --symbol BTCUSDT --interval 1d --start 2020-01-01 --end 2025-12-18 --fractional --debug`

With optimization:  

`python run_backtest.py --symbol ETHUSDT --interval 4h -o`

Force download and no plot:  

`python run_backtest.py --symbol BTCUSDT --interval 1h -f --no-plot`

Live Alerts:  

`python run_alert.py --symbol BTCUSDT --interval 30m --test-alert --verbose`

Simple alert mode:  

`python run_alert.py --symbol SOLUSDT --interval 1h`

## Deployment Ke Raspberry Pi

Copy the folder over, set up venv, install requirements.

Set Telegram env vars.

Create systemd service and timer.
I run it with systemd timer—every 5 minutes for 5m chart. Survives reboots, network drops.

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
[6]: https://i.imgur.com/rhqtB4C.png
[7]: https://kernc.github.io/backtesting.py/doc/backtesting/