---
title: 'VectorBT - Berdagang Tanpa Emosi'
date: 2026-02-01 21:09:05
updated_at: 2026-05-16 20:43:11
tags:
- tutorial
- trading
---

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
- [Risk management layer](#risk-management-layer)
  - [Stop loss](#stop-loss)
  - [Leverage settings](#leverage-settings)
  - [Position sizing](#position-sizing)
  - [Duplicate order prevention](#duplicate-order-prevention)
- [Logging dan debugging](#logging-dan-debugging)
  - [Simpan semua signal](#simpan-semua-signal)
  - [Logging response exchange](#logging-response-exchange)
  - [API failure dan reconnect issue](#api-failure-dan-reconnect-issue)
  - [Partial fill dan synchronization problem](#partial-fill-dan-synchronization-problem)
  - [Latency dan slippage](#latency-dan-slippage)
- [Telegram notification system](#telegram-notification-system)
  - [Entry dan exit alert](#entry-dan-exit-alert)
  - [Error notification](#error-notification)
  - [Balance dan position update](#balance-dan-position-update)
  - [Kenapa Telegram digunakan](#kenapa-telegram-digunakan)
- [Konklusi](#konklusi)

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

```
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

## Risk management layer

Salah satu kesilapan paling besar dalam automated trading ialah terlalu fokus kepada entry strategy tetapi mengabaikan risk management.

Ramai boleh bina strategy yang nampak profitable dalam backtest. Tetapi bila strategy tersebut masuk ke live market, masalah sebenar mula muncul. Satu trade buruk, leverage terlalu besar, atau bug kecil dalam execution layer sudah cukup untuk menghapuskan sebahagian besar account.

Disebabkan itu, dalam sistem automated trading, risk management bukan sekadar “tambahan”. Ia sebenarnya sebahagian daripada core system.

Dalam project ini, risk management layer diletakkan di antara strategy signal dan execution engine.

```
Strategy Signal
        ↓
Risk Management Layer
        ↓
Execution Engine
        ↓
Exchange
```

Ini bermaksud, walaupun strategy menghasilkan signal entry, order tersebut masih perlu melalui beberapa validation terlebih dahulu sebelum dihantar ke exchange.

Sebagai contoh:

* Semak sama ada position masih terbuka
* Semak jumlah margin tersedia
* Semak leverage
* Elak duplicate order
* Hadkan jumlah position serentak
* Cooldown selepas kerugian berturut-turut

Tujuan utama layer ini adalah untuk mengelakkan strategy daripada melakukan tindakan yang terlalu agresif ketika market bergerak tidak menentu.

### Stop loss

TODO

### Leverage settings

TODO

### Position sizing

Antara perkara paling penting dalam risk management ialah position sizing.

Ramai trader terlalu fokus mencari “entry terbaik”, sedangkan saiz position sebenarnya lebih memberi kesan kepada survival account dalam jangka panjang.

Sebagai contoh, dalam sistem ini aku lebih suka menggunakan fixed risk percentage berbanding fixed quantity.

```python
risk_per_trade = 0.01
position_size = balance * risk_per_trade
```

Dalam contoh di atas, setiap trade hanya mempertaruhkan 1% daripada balance semasa.

Pendekatan ini membantu memastikan account masih boleh bertahan walaupun mengalami beberapa loss berturut-turut.

Kalau position sizing terlalu besar, strategy yang sebenarnya profitable pun boleh musnah hanya kerana volatility jangka pendek.

### Duplicate order prevention

Dalam live trading, kadang-kadang signal yang sama boleh terhasil beberapa kali akibat delay data, reconnect websocket, atau issue synchronization.

Kalau tidak dikawal, bot mungkin menghantar duplicate order tanpa disedari.

Disebabkan itu, execution layer perlu menyemak sama ada position untuk sesuatu symbol sudah wujud sebelum membuka trade baru.

```python
if symbol in active_positions:
    skip_entry()
```

Benda seperti ini nampak kecil, tetapi dalam live trading, bug kecil seperti duplicate order boleh menjadi sangat mahal.

**Automation bukan sekadar auto entry**

Ramai menganggap automated trading hanyalah tentang “buka trade secara automatik”.

Sebenarnya, bahagian paling penting dalam automation ialah bagaimana sistem mengawal risiko ketika market bergerak di luar jangkaan.

Signal entry mungkin datang daripada strategy.

Tetapi survival account dalam jangka panjang biasanya lebih bergantung kepada risk management layer.

## Logging dan debugging

Salah satu perkara yang cepat disedari bila mula membina live trading system ialah market sebenar jauh lebih “messy” berbanding backtest.

Dalam backtest, semuanya kelihatan kemas:

* Candle sentiasa lengkap
* Order sentiasa filled
* Tiada latency
* Tiada API failure
* Tiada reconnect issue

Tetapi dalam live environment, banyak perkara boleh berlaku di luar jangkaan.

Kadang-kadang exchange lambat memberi response.
Kadang-kadang websocket terputus.
Kadang-kadang order hanya partially filled.
Kadang-kadang API langsung tidak memberi response.

Disebabkan itu, logging menjadi antara komponen paling penting dalam automated trading system.

Tanpa logging yang baik, sangat sukar untuk mengetahui apa sebenarnya berlaku ketika bot membuat sesuatu keputusan.

### Simpan semua signal

Dalam project ini, setiap signal yang dihasilkan oleh strategy disimpan ke dalam log.

Contohnya:

* Masa signal terhasil
* Symbol
* Harga semasa
* Jenis signal
* Indicator value
* Timeframe

Contoh mudah:

```python
logger.info({
    "symbol": symbol,
    "signal": "long",
    "price": close_price,
    "rsi": current_rsi
})
```

Tujuan utama bukan sekadar untuk debugging, tetapi juga untuk audit trail.

Kadang-kadang kita rasa strategy “buat silap”, tetapi selepas semak log baru sedar sebenarnya signal tersebut memang valid berdasarkan keadaan market ketika itu.

### Logging response exchange

Selain signal strategy, response daripada exchange juga perlu direkodkan.

Contohnya:

* Order ID
* Filled quantity
* Entry price
* Fees
* Status order
* Error response

Sebagai contoh:

```python
logger.info({
    "order_id": order["id"],
    "status": order["status"],
    "filled": order["filled"]
})
```

Ini sangat membantu bila berlaku masalah seperti:

* Order tidak filled sepenuhnya
* Position size tidak tepat
* Order rejected
* Trade tidak muncul dalam portfolio

Dalam live trading, kadang-kadang masalah bukan datang daripada strategy, tetapi daripada execution layer atau exchange itu sendiri.

### API failure dan reconnect issue

API exchange tidak sentiasa stabil.

Ada masa exchange mengalami downtime.
Ada masa request timeout.
Ada masa websocket disconnect secara tiba-tiba.

Kalau bot tidak mengendalikan keadaan ini dengan betul, ia boleh menyebabkan:

* Missing signal
* Duplicate order
* Position tidak synchronize
* Trade execution delay

Disebabkan itu, error handling dan reconnect logic sangat penting.

Contoh mudah:

```python
try:
    ticker = exchange.fetch_ticker(symbol)

except Exception as e:
    logger.error(e)
    reconnect()
```

Dalam production environment, retry mechanism biasanya lebih kompleks dan menggunakan exponential backoff untuk mengelakkan spam request terhadap exchange.

### Partial fill dan synchronization problem

Satu lagi masalah biasa dalam live trading ialah partial fill.

Dalam backtest, kita biasanya assume order terus filled sepenuhnya.

Tetapi dalam live market, order mungkin hanya filled sebahagian:

```
Order size: 1 BTC
Filled: 0.4 BTC
Remaining: 0.6 BTC
```

Kalau bot tidak mengendalikan keadaan ini dengan betul, internal portfolio state boleh menjadi tidak tepat.

Contohnya:

* Bot fikir position sudah penuh
* Tetapi exchange sebenarnya baru filled sebahagian

Akibatnya:

* Stop loss salah
* Position sizing rosak
* Duplicate entry berlaku

Disebabkan itu, synchronization antara local state dan exchange state perlu sentiasa diperiksa.

### Latency dan slippage

Dalam backtest, signal dan execution biasanya berlaku pada harga yang sama.

Tetapi dalam live market, ada delay kecil antara:

* Signal generation
* API request
* Exchange execution

Delay beberapa ratus millisecond mungkin nampak kecil, tetapi dalam market volatile, harga boleh berubah dengan cepat.

Ini dipanggil slippage.

Contohnya:

```
Signal price: 100
Actual fill price: 101.2
```

Perbezaan kecil seperti ini boleh memberi kesan besar terhadap performance strategy, terutama untuk scalping atau high frequency setup.

Disebabkan itu, live performance hampir tidak pernah sama sepenuhnya dengan backtest result.

**Logging bukan sekadar untuk debugging**

Pada awalnya aku anggap logging hanya penting untuk cari bug.

Tetapi semakin lama project ini berkembang, semakin jelas bahawa logging sebenarnya adalah “memori” kepada keseluruhan trading system.

Tanpa log:

* Sukar trace kenapa trade berlaku
* Sukar reproduce bug
* Sukar audit performance
* Sukar bezakan strategy issue dan execution issue

Dalam automated trading, banyak masalah hanya muncul selepas sistem berjalan berhari-hari atau berminggu-minggu.

Dan selalunya, satu-satunya cara untuk memahami apa yang berlaku ialah melalui log yang disimpan oleh sistem tersebut.

## Telegram notification system

Dalam sistem live trading, kita tidak boleh bergantung kepada pemantauan manual secara berterusan. Walaupun strategy berjalan automatik, keadaan sistem dan execution masih perlu dipantau secara aktif.

Di sinilah Telegram notification system digunakan sebagai medium komunikasi utama antara bot dan pengguna.

Fungsi utama sistem ini bukan untuk “memantau market”, tetapi untuk memberi gambaran segera tentang apa yang sedang dilakukan oleh bot pada tahap execution.

### Entry dan exit alert

Setiap kali trade dibuka atau ditutup, sistem akan menghantar notifikasi terus ke Telegram.

Contohnya:

* Signal entry long/short
* Harga entry
* Symbol
* Saiz position
* Status order

Contoh struktur payload:

```python
message = {
    "symbol": symbol,
    "side": "buy",
    "price": entry_price,
    "size": position_size
}
```

Notifikasi ini penting untuk memberikan visibility terhadap setiap keputusan yang dibuat oleh sistem.

Walaupun trade dilakukan secara automatik, setiap entry masih boleh disemak secara manual melalui log Telegram.

### Error notification

Selain trade event, sistem juga menghantar alert apabila berlaku error di execution layer.

Ini termasuk:

* API request failure
* Order rejected
* Timeout response
* Exchange error message
* Unexpected exception dalam bot

Contoh:

```python
send_telegram_alert({
    "type": "error",
    "message": str(e),
    "context": "create_order"
})
```

Tujuan utama alert jenis ini ialah untuk memastikan masalah kritikal tidak berlaku secara senyap tanpa disedari.

Dalam live trading, silent failure lebih berbahaya berbanding error yang terus dilaporkan.

### Balance dan position update

Sistem juga boleh menghantar update apabila terdapat perubahan pada position atau balance selepas execution.

Contohnya:

* Position dibuka
* Position ditutup
* Unrealized PnL berubah secara signifikan

Ini membantu memastikan state bot selari dengan keadaan sebenar di exchange, terutamanya apabila berlaku partial fill atau delay synchronization.

### Kenapa Telegram digunakan

Telegram dipilih kerana:

* API mudah digunakan
* Response cepat
* Sokongan message real-time
* Boleh digunakan tanpa setup infrastructure tambahan
* Boleh digunakan di desktop dan mobile secara serentak

Ini menjadikannya sesuai sebagai notification layer untuk sistem trading yang ringan dan self-hosted.

Walaupun execution berlaku sepenuhnya dalam bot, Telegram bertindak sebagai lapisan kesedaran luar (external awareness layer).

Ia tidak mengubah logic trading, tetapi memberikan visibility terhadap:

* Apa yang sedang dilakukan oleh sistem
* Apa yang telah berlaku dalam market melalui bot
* Sama ada sistem masih berfungsi seperti yang diharapkan

Dalam konteks automation, ini penting kerana sistem tidak boleh diperhatikan secara langsung sepanjang masa.

Telegram menjadi cara paling ringkas untuk “melihat” apa yang sedang berlaku tanpa perlu membuka codebase atau dashboard kompleks.

## Konklusi

Peralihan daripada backtest kepada live trading bukan sekadar menukar data historical kepada real-time data. Ia sebenarnya perubahan kepada cara sistem beroperasi sepenuhnya.

Dalam backtest, fokus utama adalah pada prestasi strategy — bagaimana entry dan exit menghasilkan profit berdasarkan data lepas. Tetapi dalam live environment, fokus itu berubah kepada kebolehpercayaan sistem, kestabilan execution, dan ketahanan terhadap keadaan market yang tidak konsisten.

Apa yang kelihatan mudah di atas kertas, menjadi lebih kompleks apabila berhadapan dengan realiti seperti latency, slippage, API failure, partial fill, dan ketidaksinkronan data antara strategy dan exchange. Semua ini tidak wujud dalam backtest, tetapi memberi kesan langsung kepada hasil trading sebenar.

Dalam konteks ini, automation tidak bermaksud menghapuskan risiko atau menjamin keuntungan. Ia lebih kepada mengurangkan campur tangan emosi dalam proses execution. Entry dan exit tidak lagi bergantung kepada rasa takut atau tamak, tetapi kepada set aturan yang telah ditentukan lebih awal.

Namun begitu, sistem automasi hanya sekuat komponen yang menyokongnya. Tanpa risk management yang jelas, tanpa logging yang boleh dipercayai, dan tanpa mekanisme alert yang konsisten, automated trading boleh menjadi lebih berbahaya daripada trading manual kerana ia mampu melakukan kesilapan secara berulang tanpa intervensi manusia.

Akhirnya, live trading bukan tentang membina sistem yang sempurna. Ia tentang membina sistem yang cukup stabil untuk bertahan dalam ketidaksempurnaan market sebenar, sambil terus memberi maklumat yang cukup untuk difahami, dianalisis, dan diperbaiki dari masa ke masa.

[VectorBT Live Signal](https://t.me/+Uajmc5lMbf3nYiJW) Telegram channel.

[1]: https://luangdiri.github.io/2021/08/21/perdagangan-algoritma-bhg-1.html
[2]: https://luangdiri.github.io/2026/01/04/vectorbt-backtest-alert.html
[ccxt]: https://github.com/ccxt/ccxt