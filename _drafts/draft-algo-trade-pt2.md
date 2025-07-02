---
title: "Perdagangan Algoritma -- Bhg. 2"
date: 2025-07-02 16:31:00
lastmod: 2025-07-02 16:30:23
tags:
- tutorial
---

Bahagian 1 post ini telah membincangkan tentang pramuka perdagangan algoritma. Untuk bahagian 2, kita akan bincangkan macam mana nak mengaplikasikan teori strategi dalam bentuk code.

## 1. Live Trading

Dah puas backtest dan hasilnya memuaskan? Masa untuk automate dan live trade. Di sinilah programming betul-betul main peranan.

### 1.1 Bahasa & Tools

* **Python** (paling popular)
* **Node.js / JavaScript**
* **C/C++** (kalau nak lebih low-level dan laju)
* Library popular:

  * `ccxt` untuk akses exchange API
  * `websockets` untuk live data
  * `pandas` untuk analisis data

### 1.2 Struktur Kod Asas Algo Trading

Selepas strategi difahami dan diuji, tibalah masa untuk **automate trading secara live**. Tapi sebelum pergi jauh, mari kita faham dulu **struktur asas algo trading**.

#### Struktur Umum Algo Trading (Langkah demi langkah)

1. **Ambil data sejarah (historical)** – untuk kira indikator dan setup awal
2. **Stream data baru (real-time candles)** – supaya strategi boleh terus update
3. **Periksa syarat strategi setiap candle baru**
4. **Letakkan order jika syarat dipenuhi**
5. **Logkan aktiviti algo (signal, trade, error)** – penting untuk debugging & analisis prestasi

```python
# pseudo-code
get_historical_data()
stream_new_candles()
while True:
    check_if_entry_conditions_met()
    if met:
        place_trade()
    log_all_actions()
```

#### Contoh Strategi: Moving Average Crossover

**Strategi:**

* Bila **MA 50** potong ke atas **MA 200** → **Buy signal**
* Bila **MA 50** potong ke bawah **MA 200** → **Sell signal**

Ini adalah strategi simple, tapi sangat berkuasa bila dipadankan dengan market trending.

Kita akan guna `pandas`, `ccxt` (jika nak guna live data), dan `ta` untuk kira indikator. Di bawah ini contoh **pseudo-live script** yang boleh diguna untuk signal detection dan entry.

```python
import pandas as pd
import ccxt
import time
from ta.trend import SMAIndicator

# 1. Setup exchange (guna Binance sebagai contoh)
exchange = ccxt.binance({
    'enableRateLimit': True,
})

symbol = 'BTC/USDT'
timeframe = '1h'
limit = 500

# 2. Function untuk dapatkan data dan kira MA
def fetch_data():
    ohlcv = exchange.fetch_ohlcv(symbol, timeframe, limit=limit)
    df = pd.DataFrame(ohlcv, columns=['timestamp', 'open', 'high', 'low', 'close', 'volume'])
    df['timestamp'] = pd.to_datetime(df['timestamp'], unit='ms')
    
    df['ma50'] = SMAIndicator(df['close'], window=50).sma_indicator()
    df['ma200'] = SMAIndicator(df['close'], window=200).sma_indicator()
    
    return df

# 3. Function untuk detect crossover
def check_crossover(df):
    latest = df.iloc[-1]
    prev = df.iloc[-2]
    
    if prev['ma50'] < prev['ma200'] and latest['ma50'] > latest['ma200']:
        return "BUY"
    elif prev['ma50'] > prev['ma200'] and latest['ma50'] < latest['ma200']:
        return "SELL"
    else:
        return "HOLD"

# 4. Simulasi pseudo-live loop
while True:
    df = fetch_data()
    signal = check_crossover(df)
    
    print(f"[{df.iloc[-1]['timestamp']}] Signal: {signal}")
    
    # Di sini kau boleh tambah fungsi letak order (execute_trade(signal))
    # atau simpan dalam database / log file

    time.sleep(60 * 60)  # tunggu 1 jam untuk candle baru (ikut timeframe)
```

**Perkara yang Boleh Ditambah:**

* Logging ke fail `.csv` atau database (untuk audit dan analisis)
* Position management (check kalau dah masuk trade, jangan masuk lagi)
* Take Profit & Stop Loss (boleh automate ikut ATR atau %)
* Integrasi order execution: `exchange.create_market_buy_order()` atau `create_limit_order()`

**Nota Penting Sebelum Live Trade**

* **Test dulu** dengan small capital atau di mode demo (paper trading)
* Guna `try/except` block untuk tangani error semasa API call
* Pastikan ada **logging** untuk semak error dan debug
* Tambah `check_balance()` sebelum place order