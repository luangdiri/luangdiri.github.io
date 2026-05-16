---
title: 'VectorBT - Berdagang Tanpa Emosi'
date: 2026-02-01 21:09:05
updated_at: 2026-02-01 21:09:09
tags:
- tutorial
- trading
---

*WIP*

*[Bahagian I][1] - Perdagangan Algoritma*  
*[Bahagian II][2] - VectorBT & Backtest Alert*

Dalam [post][2] yang terakhir kita sudah terokai bagaimana VectorBT sebagai backtest engine berfungsi dan menggunakan signal yang terhasil untuk dihantar ke Telegram sebagai alert berkala.

Dalam post kali ini, aku akan membincangkan tentang *live trading* pula yang mengintegrasikan platform seperti Binance dan Hyperliquid untuk melaksanakan sesebuah trade.

Seperti sedia maklum, signal yang terhasil dari VectorBT boleh digunakan untuk menghantar alert ke Telegram. Dalam masa yang sama, kita juga boleh inject function selepas alert tersebut iaitu memulakan trade di exchange pilihan. Dalam post ini aku akan cerita macam mana nak membuka trade di Binance menggunakan package Python iaitu [CCXT][ccxt].

---

**Kandungan:**

- [Ulangkaji](#ulangkaji)
- [Live trade](#live-trade)
  - [Asas bina live trade](#asas-bina-live-trade)

---

## Ulangkaji

[Sebelum ini][2] kita dah pun bina asas untuk engine backtest strategy iaitu [MACrossStrategy](https://github.com/luangdiri/vectorbt-demo/blob/master/core/macross.py). Dari class ini kita dapat hasilkan data backtest seperti *long/short entry* dan *exit*. Dengan data yang terhasil ini, kita dapat extend capability nya untuk menghantar alert ke Telegram. Alert ini di hantar secara berkala menggunakan *systemd service* dan *timer* dalam sebuah Raspberry Pi.

![vbt-no-trade](https://i.imgur.com/cA2j0MM.png)

Rajah di atas menunjukkan carta aliran framework VectorBT dengan alert.

Bermula dari *modules*, script backtest atau alert di jalankan melalui CLI:

```bash
# backtest
python .\run_backtest.py --strategy macross --ticker BTC/USDT --timeframe 1h --start 2025-01-01

# alert
python .\run_alert.py --strategy macross --ticker BTC/USDT --timeframe 1h
```

Dari skrip ini, kita menyatakan argument seperti *strategy, ticker (symbol) dan timeframe*. Argument ini pula disalurkan ke `get_data()` function iaitu, utiliti yang memuat turun data candlestick dari Binance. *StrategyClass* kemudian nya memproses data-data tersebut ke dalam *VectorBT Engine* untuk menghasilkan statistik backtest.

Bergantung pada script mana yang kita jalankan tadi, kalau kita `run_backtest`, ia akan melakukan proses plotting berserta statistik. Manakala `run_alert` tidak mengeluarkan chart plotting sebaliknya cuma mengambil *signal terakhir* untuk di hantar ke Telegram.

## Live trade

Aku akan menambah satu lagi argument dalam script `run_alert` iaitu `--live-trade`. Flag ini jika di-enabled akan membuat satu extension kepada sistem alert yang sedia ada iaitu memanggil API dari Binance untuk membuat buy/sell order menggunakan library [CCXT][ccxt].

```bash
# alert dengan live trade
python .\run_alert.py --strategy macross --ticker BTC/USDT --timeframe 1h --live-trade
```

Ringkasan pseudocode untuk bahagian live trade ini kelihatan seperti berikuat:

```python
if long or short:
    signal_msg = "entry long or short signal"
    if live_trade:
        live_msg = execute_live_trade() # market order placing
    send_message(signal_msg + live_msg)
```

Kalau sebelum ini, kita cuma ada `signal_msg` saja, sekarang kita inject function `execute_live_trade`. Fungsi function ini adalah untuk membuat order dalam Binance (atau mana-mana exchange yang di integrasikan) dan menghasilkan info seperti *entry price, stop loss price,* dan juga *liquidation price*.

![vbt-tg-alert-trade](https://i.imgur.com/YEQn6QV.png)

<small><i>Contoh alert bersama live trade</i></small>

### Asas bina live trade

![vbt-full](https://i.imgur.com/cZ9fM5e.png)

Kalau backtest cuma sekadar simulasi terhadap historical data, live trade pula adalah execution sebenar terhadap market semasa. Ini bermaksud, setiap signal yang terhasil bukan lagi sekadar statistik atas chart, tetapi akan terus berinteraksi dengan exchange melalui API.

Dalam framework kecil yang aku bina ni, live trade dipecahkan kepada beberapa komponen kecil:

1. Signal generation
2. Trade execution
3. Risk management
4. Logging
5. Notification

Secara kasar, aliran dia kelihatan seperti ini:

```text
Market Data
    ↓
Strategy Signal
    ↓
Trade Executor
    ↓
Exchange API
    ↓
Telegram Alert + Logging
```

Sebelum trade dihantar ke exchange, strategy akan menentukan sama ada signal semasa memenuhi syarat entry atau tidak. Contohnya dalam strategy moving average crossover:

```python
if fast_ma_above and not in_position:
    signal = "long"

elif fast_ma_below and in_position:
    signal = "exit"
```

Signal ini kemudiannya disalurkan ke execution layer.

Dalam architecture ini, aku cuba asingkan logic strategy dengan logic exchange. Strategy tidak patut tahu bagaimana order dihantar ke Binance atau Hyperliquid. Ia cuma menghasilkan signal seperti:

```python
{
    "side": "buy",
    "symbol": "BTC/USDT",
    "type": "market"
}
```

Kemudian object tersebut diproses oleh execution engine.

Ini penting supaya strategy yang sama boleh digunakan pada banyak exchange tanpa perlu rewrite keseluruhan code.

Sebagai contoh, execution layer menggunakan package Python iaitu [CCXT](https://github.com/ccxt/ccxt?utm_source=chatgpt.com) untuk menyeragamkan API exchange.

```python
import ccxt

exchange = ccxt.binance({
    "apiKey": API_KEY,
    "secret": API_SECRET,
    "enableRateLimit": True
})
```

Dengan abstraction seperti ini, code untuk membuat market order menjadi agak mudah:

```python
exchange.create_market_buy_order(
    symbol="BTC/USDT",
    amount=0.001
)
```

Atau untuk short position dalam futures:

```python
exchange.create_market_sell_order(
    symbol="BTC/USDT",
    amount=0.001
)
```

Walaupun nampak mudah, live trade sebenarnya jauh lebih kompleks berbanding backtest.

Dalam backtest, kita boleh assume order sentiasa filled pada harga tertentu. Tetapi dalam live market, ada beberapa faktor tambahan:

* Slippage
* Spread
* Latency
* Order rejection
* Insufficient balance
* Exchange downtime
* API rate limit

Disebabkan itu, execution layer perlu ada error handling.

Sebagai contoh:

```python
try:
    order = exchange.create_market_buy_order(
        symbol,
        amount
    )

except Exception as e:
    logger.error(e)
```

Kalau tidak, satu error kecil dari exchange boleh menyebabkan keseluruhan bot crash tanpa sebarang notifikasi.

Selain itu, aku juga lebih suka memisahkan process signal dan process execution. Maksudnya, strategy hanya menghasilkan signal, manakala execution engine pula menentukan sama ada signal itu valid untuk di execute atau tidak.

Ini membolehkan beberapa lapisan validation dibuat seperti:

* Semak sama ada position masih terbuka
* Semak leverage
* Semak margin balance
* Semak cooldown timer
* Elak duplicate order

Kalau semua logic ini dicampur terus ke dalam strategy class, code akan jadi terlalu coupled dan susah untuk di maintain dalam jangka masa panjang.


[VectorBT Live Signal](https://t.me/+Uajmc5lMbf3nYiJW) Telegram channel.

[1]: https://luangdiri.github.io/2021/08/21/perdagangan-algoritma-bhg-1.html
[2]: https://luangdiri.github.io/2026/01/04/vectorbt-backtest-alert.html
[ccxt]: https://github.com/ccxt/ccxt