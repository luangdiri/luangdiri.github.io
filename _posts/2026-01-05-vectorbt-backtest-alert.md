---
title: 'VectorBT Backtest & Telegram Alert'
date: 2026-01-04 14:39:47
updated_at: 2026-01-04 22:52:12
tags:
- tutorial
- trading
---

<img alt="backtesting logo" src="https://i.imgur.com/gvFn4Wn.png">

*Pergi ke [Bahagian I][2]*

Dari dulu lagi bercita-cita nak kasi trading strategy aku di automasi kan. Kali ni dengan bantuan LLM, akhirnya aku berjaya bina stack aku sendiri. Basically 90% vibe coded je projek ni. Stack yang aku bina ni takde lah advanced sangat kalau nak dibandingkan dengan stack yang pro quant pakai, tapi bagi aku dah cukup untuk permulaan seorang trader yang baru nak berkecimpung dalam dunia algo trade.

Kalau dulu aku buat dari scratch, memang berterabur code aku. Tak ada standard, kalau nak ubah parameter kena ubah semua file. Sekarang bila dah tahu konsep sebenar, baru boleh atur dengan jelas. 

Basically nak bina stack backtest ni, langkah pertama kena ada strategy yang kita cipta atau jumpa di internet. Dalam post ni pun aku akan cerita pasal strategy MA crossover biar senang nak cerita. Aku juga ada buat alert untuk signal tu di hantar ke Telegram. Untuk run 24/7, aku host script ni dalam Raspberry Pi.

---
**Kandungan:**

- [Permulaan](#permulaan)
  - [Apa itu backtest?](#apa-itu-backtest)
  - [Prosedur operasi](#prosedur-operasi)
  - [Stack](#stack)
- [Strategi](#strategi)
  - [Moving Average Cross (MA Cross)](#moving-average-cross-ma-cross)
  - [Historical price data](#historical-price-data)
  - [Backtest](#backtest)
  - [Telegram alert](#telegram-alert)
  - [Punca signal](#punca-signal)
- [Backtest result](#backtest-result)
  - [Raw data](#raw-data)
  - [Statistik](#statistik)
  - [Plotting](#plotting)
- [Deployment Ke Raspberry Pi](#deployment-ke-raspberry-pi)
  - [scp](#scp)
  - [venv](#venv)
  - [cronjob](#cronjob)
  - [systemd service dan timer](#systemd-service-dan-timer)
  - [Routine check](#routine-check)
- [Penutup](#penutup)


---

## Permulaan

### Apa itu backtest?

Berbalik kepada cerita asal, backtesting adalah satu fasa dalam sistematik trading iaitu menguji strategi dengan menggunakan data harga di masa lampau. Kita nak uji strategi tersebut berkesan atau tidak bergantung kepada keadaan market. Struktur market seperti yang kita tahu, ada beberapa fasa iaitu, *uptrend, sideway dan downtrend*. Ada juga orang sebut fasa ini sebagai accumulation, expansion dan distribution. Ikut la mana suka, konteks nya tetap sama.

Bila kita uji sesebuah strategi tu, kita cuma melihat performance nya dengan data yang lampau. Bererti ujian tu mungkin boleh perform atau tak boleh diterapkan untuk perjalanan harga di masa depan. Kata lain, past performance tak menjamin future performance. Itu sebab kita kena backtest strategi tersebut dengan data sample yang banyak.

Data sample dalam hal ini merujuk kepada data sejarah harga dari awal sesebuah pair itu hidup di market sehingga lah sekarang. Lagi banyak data sample yang kita dapat, lebih bermutu final result yang di keluarkan dari ujian backtest. Sebagai contoh, market XAU/USD memang banyak sebab ia dah wujud sejak 1980an lagi. BTC/USD pula paling awal tahun 2009 macam tu. 

### Prosedur operasi

Untuk permulaan, kita perlu ada strategi untuk melaksanakan proses sistematik trading ni.

1. Idea strategy
2. Convert strategy jadi code
3. Integrate code dalam framework
4. Lakukan backtest

Bila dah lengkap semua, baru lah kita boleh integrate fungsi alert di mana signal dari backtest tadi kita boleh hantar ke Telegram.

Biasa nya prosedur yang biasa aku buat bila dah dapat sesuatu strategi adalah:

1. Backtest dulu dalam TradingView -- guna Pine Script
2. Convert Pine Script ke Python -- suruh LLM buatkan
3. Integrate ke dalam python backtest framework -- dalam hal ini, aku guna **VectorBT**

**Kenapa TradingView?**

Part ni optional dan lain orang mungkin berlainan cara. Tapi sebagai seorang yang cheapskate macam aku, aku akan mulakan dengan benda yang free dulu. TradingView ada juga feature untuk backtest strategi menggunakan Pine Script (scripting language untuk TradingView). Cuma historical price data untuk free tier nya terhad kepada 5,000 bars sahaja (untuk intraday timeframe). Jadi tak syok la tak dapat guna semua data yang ada. Macam aku cakap tadi, lagi banyak data kita ada, lagi bagus result backtest.

Kadang tu bila backtest dalam TradingView ni result backtest nampak gah, profit factor >10, sharpe ratio >1, bila backtest dengan data yang banyak tiba-tiba jadi barai. Selain tu, kalau kita bina custom sendiri, banyak fleksibiliti yang kita dapat buat.

> ⚠⚠ Iklan ⚠⚠  
> Register akaun TradingView menggunakan ref link ini:  
> [https://www.tradingview.com/pricing/?share_your_love=natebroke][3]  
> Kita kan kawan :)

### Stack

Tech yang digunakan sepanjang perjalanan backtesting ni adalah:

|||
|---|---|
| **Language** | Python, Pine Script |
| **Backtest framework** | [VectorBT][1] |
| **Alert** | Telegram |
| **LLM** | Grok, ChatGPT |
| **Server** | Raspberry Pi 3 Model B Rev 1.2, 1GB |
| **Server OS** | Debian 12 (bookworm) |

Tak ada Raspberry Pi pun takpe, benda tu untuk kita jalankan alert bot selama 24/7. Guna laptop pun boleh, cuma kena biarkan on je lah. Option lain mungkin kena sewa mana-mana cloud hosting macam DigitalOcean atau Linode.

Untuk engine backtest, aku gunakan [VectorBT][1] framework. Ini adalah framework untuk backtest strategy trading yang menggunakan python.

Aku pilih framework ini sebab aku tengok dia *macam* lightweight, sebab feature nya tak sebanyak framework backtest lain. Mula-mula aku nak guna [Hummingbot][4], tapi learning curve dia agak curam ya. Walaupun guna LLM, aku tetap jadi bodoh dalam mengintegrasikan Hummingbot dalam sistem aku. Mungkin sebab sasaran objektif aku cuma sikit je kot iaitu nak dapatkan backtest result je. Bila dah ada banyak feature yang bloated dalam sesebuah framework tu rasa berat kepala otak.

Jadi VectorBT ni engine nya hanya fokus kepada kerja nak backtest dan buat analisis saja, live trading dan alert kena custom sendiri.

## Strategi

### Moving Average Cross (MA Cross)

Untuk post ni aku demo untuk strategi MA cross. Basically MA cross ni dia detect signal bila ada crossing antara dua MA, sama ada cross atas atau bawah. Untuk memudahkan cerita, kondisi untuk beli atau jual adalah seperti berikut:

1. **MA 14** cross atas **MA 20**: signal beli
2. **MA 14** cross bawah **MA 20**: signal jual

Simple je, demi contoh. Bila di convertkan ke Pine Script bentuk nya jadi macam ni:

**Pine Script:**

```javascript
fast_ma = sma(source, 14)
slow_ma = sma(source, 20)

// condition
longCondition = crossover(fast_ma, slow_ma)
shortCondition = crossunder(fast_ma, slow_ma)

// entry dan exit
if (longCondition)
    strategy.entry("Long", strategy.long)

if (shortCondition)
    strategy.entry("Short", strategy.short)
```

*Full code: https://gist.github.com/luangdiri/efb937e67f88e3a9ab738c5bd82535f4*

Bila dah selesa dengan Pine Script dan hasil backtest dalam TradingView, baru lah kita convert ke dalam python:

> *Nota:*  
> Nak senang, guna LLM seperti Grok atau ChatGPT untuk convert Pine Script ke Python. Kalau ada bug tu pandai-pandai lah betulkan*

```python
fast_ma = ta.trend.SMAIndicator(close, 14).sma_indicator()
slow_ma = ta.trend.SMAIndicator(close, 20).sma_indicator()

# condition
ma_bull = fast_ma > slow_ma
ma_bear = slow_ma > fast_ma

# define entry dan exit
long_entries = ma_bull
short_entries = ma_bear
long_exits = ma_bear
short_exits = ma_bull
```

### Historical price data

Untuk historical price data, dalam projek ni aku cuma ambil dari Binance spot market. Tak pasti boleh tarik dari perpetual market ke tak, belum cuba lagi. Jadi aku ada script untuk bot ni auto download price data untuk mana-mana pair dalam Binance, dari berbagai timeframe yang ada.

Script ni basically akan check cache folder dulu, kalau dah ada data yang pernah di download sebelum ni, cuma perlu append yang candle yang baru je. Aku juga ada buat option untuk *force download* data historical ni. So, run pertama script ni akan jadi perlahan sebab dia nak kena download price data dulu apa semua, bila semua di download dan disimpan, run seterusnya cuma cek kalau ada candle baru je. Laju sikit la.

Yang neat nya pakai VectorBT ni, dia dah ada integrate sekali library CCXT. Jadi nak pilih crypto exchange selain Binance pun boleh.

```python
download_kwargs = {
    "start": "2017-01-01",
    "end": "2026-01-01", # kalau tak specify end, dia akan download data sampai ke hari ini
    "symbols": "BTC/USDT",
    "timeframe": "1h",
    "exchange": "binance"
}
data = vbt.CCXTData.download(**download_kwargs)
```

### Backtest

Untuk menyenangkan aku run script backtest, aku cuma perlu run satu line saja dari CLI:

```bash
python .\run_backtest.py --strategy macross --ticker BTC/USDT --timeframe 1h --start 2025-01-01
```

Option yang ada dalam CLI argument ni adalah *strategi* yang kita nak pakai, *ticker* iaitu pair asset yang kita nak backtest, *timeframe*, dan *start* iaitu mula dari tarikh bila kita nak run backtest ni. Kerja aku cuma kena bina strategi je, lepas tu kerja download data semua framework akan handle.

Dalam command atas tu, aku akan run backtest ke atas pair *BTC/USDT* menggunakan strategi *macross*. Timeframe yang aku pilih adalah *1h*, data candle dari tarikh *2025-01-01*. Mudah bukan?

### Telegram alert

Sama la macam backtest, cuma run satu command line je:

```bash
python .\run_alert.py --strategy irsimax_1h --ticker BTC/USDT --timeframe 1h
```

Kedua-dua script `run_backtest.py` dan `run_alert.py` ni aku pakai file yang berbeza sebab kerja dia lain-lain, maka nya dalam command alert ni tak ada argument `--start`. Argument `--strategy` juga aku guna untuk senang switch antara strategi lain.

### Punca signal

Kedua-dua command di atas sebenarnya menggunakan satu punca signal yang sama. Bila kita buat backtest, sudah tentu dia bagi signal entry dan exit. Maka nya aku guna result backtest tersebut untuk aku send alert ke Telegram.

Code snippet di bawah menunjukkan function dari VectorBT iaitu `.from_signals()`. Function ni menerima parameter seperti condition untuk long entry, short entry dan exit nya. Kita juga boleh tetapkan condition untuk *stop loss* dan *take profit*. Kalau tengok full parameter dia pun memang banyak. Kita boleh tetapkan ciri-ciri pyramiding, trailing stop, partial entry/exit, etc. Cuma sekarang aku pakai yang basic je.

```python
pf = vbt.Portfolio.from_signals(
    close=close,
    entries=long_entries,
    exits=long_exits,
    short_entries=short_entries,
    short_exits=short_exits,
    sl_stop=sl_stop,
    tp_stop=tp_stop,
    init_cash=self.initial_cash,
    freq=self.timeframe,
    accumulate=False
)
return pf
```

## Backtest result

### Raw data

Bila kita dah supply segalanya ke dalam function tadi, VectorBT akan jalankan tugas dia sebagai engine backtest dan mengeluarkan data signal dari kondisi strategi kita tadi. Kita juga boleh plot dalam chart di mana signal itu beli atau jual. Statistik performance juga boleh dihasilkan.

Untuk melihat rekod signal dari awal sampai akhir dalam bentuk table, kita perlu menggunakan `pf.trades.records_readable`. Outputnya adalah seperti berikut:

```
Exit Trade Id  Column      Size           Entry Timestamp  Avg Entry Price  Entry Fees  ... Exit Fees          PnL    Return  Direction  Status Position Id
0               0       0  0.100569 2025-01-15 14:00:00+00:00         99434.41         0.0  ...       0.0  -182.584681 -0.018258       Long  Closed           0
1               1       0  0.094398 2025-01-17 15:00:00+00:00        104000.00         0.0  ...       0.0  -110.514833 -0.011257       Long  Closed           1
2               2       0  0.092028 2025-01-30 04:00:00+00:00        105478.00         0.0  ...       0.0  -104.569265 -0.010773       Long  Closed           2
.
.
.
17             17       0  0.104911 2025-10-16 16:00:00+00:00        109232.43         0.0  ...       0.0  -124.809319 -0.010891      Short  Closed          17
18             18       0  0.102812 2025-10-30 05:00:00+00:00        110248.29         0.0  ...       0.0  -114.988158 -0.010145      Short  Closed          18
19             19       0  0.103984 2025-11-03 04:00:00+00:00        107900.37         0.0  ...       0.0  1721.854530  0.153465      Short  Closed          19
20             20       0  0.141850 2026-01-04 01:00:00+00:00         91235.37         0.0  ...       0.0   174.781753  0.013505       Long    Open          20
```

Banyak info yang boleh kita dapat dari table ni seperti waktu entry, waktu exit, PnL, direction (long atau short) dan juga status position tersebut (masih open atau sudah closed). Dari table ni la kita ambil info-info untuk kita send ke Telegram. Yang paling penting adalah last row tu, kita perlu tahu position yang baru untuk kita alert ke Telegram sebab status nya masih *Open*.

Penting nya last row tu sebab bila menulis logic, kita perlukan status *Open* tersebut. Aku pernah pakai framework **backtesting.py**. Penat-penat belajar framework tu last-last dapat tahu yang dia tak keluarkan info ni. Langsung cari framework lain maka terjumpa lah VectorBT.

Berikut adalah ringkasan logic untuk send alert ke Telegram menggunakan info dari `pf.trades`:

```python
if latest_trade.Direction == "Long":
    if latest_trade.Status == "Open":
        message = "LONG ENTRY"
    elif latest_trade.Status == "Closed":
        message = "LONG EXIT"

elif latest_trade.Direction == "Short":
    if latest_trade.Status == "Open":
        message = "SHORT ENTRY"
    elif latest_trade.Status == "Closed":
        message = "SHORT EXIT"

if message:
    send_message(message)
```

### Statistik

Tadi tu adalah data raw yang dari engine framework. VectorBT juga ada mengeluarkan statistik strategi yang di backtest. 

Guna function `pf.stats()` akan keluar statistik seperti berikut:

```
Start                         2025-01-01 00:00:00+00:00
End                           2026-01-05 08:00:00+00:00
Period                                369 days 09:00:00
Start Value                                     10000.0
End Value                                  13116.508432
Total Return [%]                              31.165084
Benchmark Return [%]                          -2.048291
Max Gross Exposure [%]                            100.0
Total Fees Paid                                     0.0
Max Drawdown [%]                              17.645252
Max Drawdown Duration                  99 days 06:00:00
Total Trades                                         21
Total Closed Trades                                  20
Total Open Trades                                     1
Open Trade PnL                               174.781753
Win Rate [%]                                       20.0
Best Trade [%]                                17.586906
Worst Trade [%]                               -1.888506
Avg Winning Trade [%]                         12.361265
Avg Losing Trade [%]                          -1.276063
Avg Winning Trade Duration             58 days 20:45:00
Avg Losing Trade Duration               3 days 06:18:45
Profit Factor                                  2.317099
Expectancy                                   147.086334
Sharpe Ratio                                   0.965235
Calmar Ratio                                   1.742355
Omega Ratio                                    1.036158
Sortino Ratio                                  1.426686
dtype: object
```

Statistik ni untuk kita tahu performance strategi. Sebagai contoh, kita nak tahu *Max Drawdown* dan tempoh drawdown tersebut, ada ditunjukkan dalam statistik. Ratio-ratio yang common dalam statistik trading macam Sharpe ratio, Calmar ratio, Sortino juga ada. Jadi mudah lah nak menilai performance strategi. Kalau tak power, cari strategi lain, atau ubah config strategi.

Yang penting, result strategi kita tu masuk akal dan bukan jenis *too good to be true*. Sebab kadang tu aku nampak orang share result backtest dengan win rate lebih 90%. Dalam proses pembinaan strategi kita kena tahu isu kritikal macam *repainting*.

> **Repainting** in backtesting is when a trading indicator or strategy changes its past historical signals or data points as new market data comes in. This creates a misleading, artificially perfect historical performance that would have been impossible to replicate in live trading. 

### Plotting

Selain data raw dan statistik, VectorBT boleh plot position entry dan exit dengan menggunakan function `pf.plot().show()`:

![irsimax_1h-2025-01-01.png](https://i.imgur.com/0h5wToL.png)

Plot ni dia tunjuk tiga chart, 1) Orders, 2) Trade PnL dan 3) Cumulative Returns. Dalam chart Orders, kita boleh lihat di mana position buy (long) atau sell (short). Dalam Orders ni dia tak tunjuk exit dekat mana, kita kena rujuk pada chart Trade PnL. Dimana ada bulat merah tu adalah *closed order* yang loss, bulat hijau adalah *closed order* yang profit. Bulat kuning menunjukkan yang position terakhir tu masih terbuka. Saiz bulat tu pun menunjukkan berapa percent return dari position yang closed tu. Untuk cumulative returns, kita boleh lihat performance PnL strategi dan dibandingkan dengan pergerakan harga asset tersebut. 

`plot()` function ni untuk basic plot je. Kalau nak custom plot pun boleh. Contoh nya nak keluarkan chart RSI atau guna candlestick, kita kena bina sendiri plot object dan define axis semua. Rumit, aku dah cuba. Plot basic ni pun dah cukup buat kegunaan aku setakat ni.

## Deployment Ke Raspberry Pi

Bahagian ni kita hanya menggunakan fungsi alert saja. Script untuk [backtest](#backtest) tadi tak diperlukan lagi. Backtest hanya berlaku dalam environment yang ada GUI sebab kita memerlukan ciri plotting. Manakala untuk [alert](#telegram-alert) kita tak perlukan GUI sebab kerja nya hanya untuk scan signal dan send alert ke Telegram. Jadi kalau nak pakai Raspberry Pi, disarankan untuk set OS tu menjadi *"headless"*.

Sebelum aku deploy ke Raspberry Pi, aku akan pastikan Raspberry Pi aku tu dah enable akses SSH supaya senang nak akses dari tempat lain. Aku tak open port apa-apa, cuma akses guna local network je. Dengan cara ni aku hanya boleh akses dalam wifi network yang sama. Bila dekat luar tak boleh masuk. Boleh je nak buka port router tu untuk masuk network tapi aku takut kalau ada lubang sekuriti kang orang lain boleh ceroboh masuk pula internet rumah aku.

Bila dah enable SSH tu, baru lah boleh copy projek VectorBT ni masuk dalam Raspberry Pi. Sebelum buat kerja meng-copy ni, kena lah backtest secara mendalam strategi dalam laptop. Lepas dah puas hati dengan hasil backtest, baru lah pindah masuk ke dalam Raspberry Pi tersebut.

Ada dua cara nak copy:

1. Pakai git
2. Pakai command `scp`

Cara pakai git paling senang, aku cuma kena push projek tu dalam Github dan terus `git clone` dari Raspberry Pi. Jadi tak perlu dokumentasi kot. 

### scp

Sebelum tu kena enable SSH akses dulu baru boleh guna command ni. `scp` command ni fungsi dia untuk transfer file dari network lain ke network lain. Jadi setiap kali ada buat code changes, aku kena buat command ni untuk transfer file baru. Rumit. Pakai git lagi senang.

Caranya transfer dari laptop ke Raspberry Pi (SSH):

1. zip kan projek folder VectorBT
2. Lakukan command ini:

```bash
scp vectorbt.zip raspi@hostname_or_ip:/home/raspi/vectorbt
```

Kalau taknak zip, terus je copy folder projek masuk ke Raspberry Pi guna command ni:

```bash
scp -r /path/to/vectorbt raspi@hostname_or_ip:/home/raspi/vectorbt
```

### venv

Dah settle tu, akses Raspberry Pi dan masuk ke direktori `/vectorbt` dan set up `venv` -- sebuah virtual environment untuk projek python.

Bila kita pakai venv ni, segala package python yang kita download hanya wujud dalam direktori projek saja. Jadi Raspberry Pi kita tu takde lah bloated dengan python package yang merepek-repek. Kalau kita hapus folder projek tu, python package yang dah di download tu semua akan terhapus juga. Ia juga memang disarankan kalau pakai komputer skala kecil ni sebab storage tak besar, memory pun tak banyak.

Cara untuk initialize dan activate `venv`:

```bash
python -m venv venv               # venv init in folder
source venv/bin/activate          # activate venv in folder
# already inside venv session                      
```

Bila dah activate, kita akan masuk ke dalam environment venv direktori projek. Dekat command tu akan dimulakan dengan `(venv)`. Dalam environment venv ni kita install package yang diperlukan oleh projek VectorBT:

```bash
pip install --upgrade pip  # optional
pip install -r requirements.txt
# exit venv
deactivate
```

Sebelum deactivate venv environment tu, test dulu script alert jalan ke tak. Sebab package tadi hanya wujud dalam environment ni. Kalau kita keluar (deactivate) dari venv, python tak boleh jumpa package tersebut dan akan keluar error "module not found".

Bila dah selesai download package dan script berjalan dengan sempurna, baru lah kita set script alert supaya dia run secara berkala.

Enter cron dan systemd timer.

### cronjob

Aku dulu memang pakai cronjob je untuk semua script yang ada dalam cloud server, sebab paling senang nak set. Tapi bila tahu ada service yang lebih sempurna dan fleksibel macam systemd timer, aku pakai itu je. Mungkin kalau projek kecil dan tak perlukan convenience macam logging, boleh la pakai cronjob. Tapi aku akan tetap buat nota sini.

Akses cronjob file dengan menggunakan command `crontab -e` dan masukkan line ini untuk run alert script kita:

```bash
*/5 * * * * /home/raspi/vectorbt/venv/bin/python /home/raspi/vectorbt/run_alert.py --strategy macross --ticker BTC/USDT --timeframe 5m 2>&1
```

Lepas tu save file crontab ni dengan menekan `ctrl+X` dan `Y` untuk save.

Command cron ini akan menjalankan script secara berkala setiap 5 minit (`*/5 * * * *`) sebab strategi kita memerlukan timeframe 5m. Jadi setiap 5 minit cron akan run script, download candle baru dan jalan kan backtest. Kalau ada signal, send alert ke Telegram.

Dari command tu, kita menggunakan python yang ada dalam `venv` dan bukan dari system OS. Ini kerana library projek tu wujud dalam folder venv tu dan hanya dalam environment venv saja dia boleh detect.

### systemd service dan timer

Cara ni aku guna sekarang untuk projek VectorBT ni. Nak setup dia agak rumit tapi bila dah tahu proses nya, jadi senang je.

Pertama sekali, kita kena setup service untuk run alert script tadi. Cipta satu file baru dalam `/etc/systemd/system/`:

```bash
sudo nano /etc/systemd/system/backtest-macross.service
```

`backtest-macross.service` adalah nama file service. Di dalam file ni kita akan letak command untuk run alert script macam dalam cron tadi. Kita juga akan set Telegram token dan Telegram channel ID untuk send ke Telegram tu berfungsi dengan baik.

Dalam file service ini, masukkan script ini:

```bash
[Unit]
Description=Telegram Alert MA Cross
After=network-online.target

[Service]
Type=oneshot
ExecStart=/home/raspi/vectorbt/venv/bin/python /home/raspi/vectorbt/run_alert.py --strategy macross --ticker BTC/USDT --timeframe 5m
WorkingDirectory=/home/raspi/vectorbt/
Environment=TELEGRAM_TOKEN=12345677:ABCD-cUasknKksdfIasdmO938d
Environment=TELEGRAM_CHAT_ID=-100288400284
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target
```

Kalau nak tahu lebih lanjut fungsi-fungsi systemd service ni, Google je lah. Yang aku tahu `Environment` tu akan dipakai oleh systemd untuk replace segala benda yang menggunakan `os.getenv()`. Lepas tu `Restart` dan`RestartSec` tu untuk bagitau yang service ni kena tunggu 10 saat kalau system reboot atau masalah network (`on-failure`). Entah lah, ikut suka aku je.

Setelah dah buat file service, cipta pula file `.timer` dalam direktori dan nama yang sama. Cuma tukar `.service` jadi `.timer` je.

```bash
sudo nano /etc/systemd/system/backtest-macross.timer
```

Dalam file ni, masukkan script ini:

```bash
[Unit]
Description=Telegram Alert MA Cross Timer

[Timer]
OnCalendar=*:0/5
Persistent=true
Unit=backtest-macross.service

[Install]
WantedBy=timers.target
```

File ini lah yang akan menjalankan script alert tu secara berkala. Kita specify timing dekat line ini `OnCalendar=*:0/5` iaitu run setiap 5 minit. Sama je macam cron tu tadi. Cuma bila pakai systemd ni, kita kena enable dan start timer tersebut menggunakan command.

```bash
# Enable and start timer
sudo systemctl enable backtest-macross.timer
sudo systemctl start backtest-macross.timer
```

Kita tak perlu enable `.service` file sebab file timer ini yang akan gunakan service file itu nanti.

Kalau nak edit file `.timer` atau `.service` tu, aku biasa akan stop dan disable dulu timer. Command nya adalah:

```bash
# To stop and disable
sudo systemctl stop backtest-macross.timer
sudo systemctl disable backtest-macross.timer
```

Boleh je kalau nak directly edit file tu, tapi lepas dah edit tu kena reload balik:

```bash
# Reload daemon after editing
sudo systemctl daemon-reload
```

Jadi kalau ada apa-apa hal macam nak ubah code strategi ke, stop dan disable dulu timer. Lepas tu guna command untuk enable dan start balik timer tersebut.

Misal kata nak hapus segala `.timer` dan `.service` file, guna command:

```bash
# To delete service and timer files from the system:
sudo systemctl stop backtest-macross.service backtest-macross.timer
sudo systemctl disable backtest-macross.timer
sudo rm /etc/systemd/system/backtest-macross.*
sudo systemctl daemon-reload
```

Untuk nak lihat status dan check log script ni jalan ke tak, guna command:

```bash
# To check status of a timer:
systemctl status backtest-macross.timer
systemctl list-timers --all

# To check logs for a service:
journalctl -u backtest-macross.service -f  # follow live
journalctl -u backtest-macross.service     # all logs
journalctl -p err -xb                      # Check for system-wide error
```

### Routine check

Untuk rutin, command `systemctl status` dan `journalctl` ni aku selalu guna bila masuk SSH ke Raspberry Pi sebab nak tengok samada timer dan script berjalan lancar atau tak. Kadang tu rumah blackout. Bila dah ada letrik Raspberry Pi pun reboot. Jadi kena la tengok balik service nya dapat restart dengan baik ke tak.

Aku juga ada buat custom logging untuk alert ni. Disimpan dalam folder `/logs` dalam direktori projek tadi. Setiap kali masuk SSH, aku akan buat command tadi dan check log folder ni. Lepas tu delete alert log yang lama-lama kasi storage ringan. Gitu je lah.

## Penutup

Jadi kerja aku lepas ni hanyalah study strategi baru, terapkan dalam VectorBT, uji dengan backtest dan jalankan alert. Simple, mudah dan boleh dipercayai, impian terealisasi.

Langkah seterusnya, mungkin boleh integrasi kan live trading pula? Library CCXT memang dah ada fungsi untuk live trading dan projek ni pula memang ada guna library tu. Mungkin Bahagian III boleh aku telusuri bidang itu. Sekain.

TODO full ma cross vectorbt source code.

[1]: https://vectorbt.dev/
[2]: https://luangdiri.github.io/2021/08/21/perdagangan-algoritma-bhg-1.html
[3]: https://www.tradingview.com/pricing/?share_your_love=natebroke
[4]: https://hummingbot.org/
[5]: https://wundertrading.com/en/register?ref=wbtdf816e60
[6]: https://i.imgur.com/rhqtB4C.png
[7]: https://vectorbt.dev/