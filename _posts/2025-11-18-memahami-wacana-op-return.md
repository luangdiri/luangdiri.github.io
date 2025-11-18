---
title: 'Memahami Wacana OP_RETURN'
date: 2025-11-18 22:26:06
updated_at: 2025-11-18 22:25:54
tags:
- blockchain
---

**Penyegaran**

Bitcoin seperti yang kita tahu, merupakan sebuah protokol matawang digital berasaskan jaringan decentralized. Ia bermula dengan terbitan sebuah whitepaper (kata lain: kertas kerja) oleh entiti yang tidak dikenali, Satoshi Nakamoto. Software Bitcoin mula di bangunkan oleh beliau dan pertama kali dijalankan pada tahun 2009. Sesetengah orang memanggil Bitcoin sebagai "matawang untuk rakyat", kerana ia tidak dikawal oleh mana-mana bank atau kerajaan. Jaringan Bitcoin tiada titik tengah untuk mengawal setiap transaksi yang berlaku. Semua lapisan masyarakat seluruh dunia boleh bergabung bersama untuk mengoperasikan rangkaian Bitcoin ini. Ia juga bersifat transparent di mana semua transaksi yang berlaku adalah terbuka untuk dilihat oleh semua orang.

Sejak [pertama kali](1) aku tulis pasal Bitcoin tahun 2014 dulu, banyak perubahan yang telah berlaku dengan teknologi ini, dari segi software, politik dan juga geopolitik. Untuk mengetahui tentang apa yang berlaku dalam scene crypto terutamanya Bitcoin pada tahun 2025 ini, boleh baca di [post ini](2).

Pada saat aku menulis post ini, market cap Bitcoin telah pun mencecah $2.2 Trillion dengan harga $114,000 per Bitcoin. Titik tertinggi harga pula mencecah $125,000 (ATH). Pada tarikh 20 April 2024, Bitcoin telah pun mengalami proses halving yang ke-4 iaitu miner reward dibelah dua menjadi 3.125 BTC setiap Bitcoin baru yang di jumpai.

- [Evolusi Bitcoin](#evolusi-bitcoin)
- [Apa itu OP\_RETURN?](#apa-itu-op_return)
- [Bitcoin Core v30](#bitcoin-core-v30)
- [Bitcoin Knots](#bitcoin-knots)
- [Kenapa peduli?](#kenapa-peduli)
- [2 sats dari aku](#2-sats-dari-aku)


## Evolusi Bitcoin

Boleh buat satu post baru kalau nak cerita semua pasal upgrade dan Bitcoin Improvement Proposal (BIP). Tapi untuk post kali ini, aku cuma fokus ke BIP yang berkaitan **OP_RETURN** saja, aku akan sampai ke point ini di bawah nanti.

Walaubagaimana pun, rajah di bawah merujuk kepada upgrade (soft forks) dan hard forks yang telah berlaku ke atas rangkaian Bitcoin sejak bermula ia dijalankan tahun 2009 sehingga tahun 2023 ([sumber](3)):

![Bitcoin forks timeline][upgrade-timeline]

Untuk rekod, aku takde lah nak cakap upgrade-upgrade ini tak bagus, ada juga baik nya dari segi privacy dan pengembangan penggunaan yang lebih meluas selain hanya berfungsi sebagai pembayaran tunai. Bila sesebuah sistem itu berasaskan sumber terbuka, maka komuniti mengalami pendapat yang berbeza. Seperti mana kita lihat dalam politik negara juga, parti-parti baru dibangunkan atas sebab pendapat yang berbeza untuk mentadbir negara.

Antara banyak-banyak upgrade di atas, dua upgrade yang paling besar adalah **Segregated Witness (SegWit)** dan **Taproot**<sup>1</sup>. Ini kerana pada waktu itu, upgrade tersebut telah mencetus kontroversi di kalangan pembangun dan komuniti Bitcoin.

**SegWit** upgrade telah membuatkan rangkaian Bitcoin berpisah kepada Bitcoin Cash (ticker: BCH)<sup>2</sup> atas sebab ketidak setujuan beberapa pihak dalam penentuan saiz block.

**Taproot** pula menjadi kontroversi kerana upgrade ini membolehkan Bitcoin menempel data non-transaksi di dalam blockchain nya, justeru membolehkan ia membawa penciptaan aplikasi seperti Ordinals dan token BRC-20. Dalam erti kata lain, Bitcoin juga boleh menjalankan smart contract seperti yang berlaku di dalam rangkaian Ethereum. Komuniti berpecah belah akibat itu kerana ia lagi menjauhkan matlamat utama Bitcoin sebagai sistem tunai elektronik dengan menyumbat data-data yang tidak diperlukan di dalam rangkaian blockchain.

## Apa itu OP_RETURN?

<blockquote class="twitter-tweet" data-theme="dark" data-dnt="true" align="center"><p lang="en" dir="ltr">Running Bitcoin Core v30</p>&mdash; Pavol Rusnak (@PavolRusnak) <a href="https://twitter.com/PavolRusnak/status/1977366567840084219?ref_src=twsrc%5Etfw">October 12, 2025</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Bayangkan sebuah transaksi Bitcoin ni sebagai satu sampul surat, kita letak duit kertas di dalam, alamat kan kepada penerima, dan hantar ke pejabat pos. Tapi macam mana kalau kita nak letak satu nota kecil dalam nya? "Jangan beli rokok dengan duit ini ya!" atau mungkin nak letak timestamp untuk kegunaan smart contract. Itu lah fungsi OP_RETURN - sebuah kod operasi (opcode, operation code) yang membolehkan kita selit data arbitrari ke dalam transaksi tersebut. Data ini tak boleh di belanja. Ia cuma sebuah metadata yang tak ada fungsi dengan baki yang ada dalam wallet.

Sejarahnya, Bitcoin Core mengehadkan data OP_RETURN kepada 80 bytes saja, untuk menghalang orang menjadikan Bitcoin blockchain ni sebagai satu hard drive. 80 bytes ni agak-agak cukup untuk buat satu Twitter post je. Kenapa? Sebab satu block transaksi hanya cukup 1 MB saja (atau 4 MB dengan SegWit). Ruangan dalam block itu berharga dan pelombong kena utamakan aliran tunai dari gambar meme jpeg.

## Bitcoin Core v30

Mara-pantas ke Oktober 2025, boom, Bitcoin Core versi 30 dikeluarkan. Headline nya? Diorang naikkan had 80 bytes tadi ke 100 KB. Itu bukan nota kecil dah, itu boleh muat sebuah buku 10 mukasurat. Nodes yang menggunakan v30 boleh perpanjangkan transaksi ke serata rangkaian, memudahkan developer untuk bina program macam yang ada dekat Ethereum smart contract.

Rasional nya? Kecekapan. OP_RETURN adalah cara "paling bersih" untuk menyimpan data dalam transaksi. Kalau sebelum ni, orang simpan data menggunakan cara "hack" iaitu menyimpan dalam UTXO atau melalui metode inscription (Ordinals), buatkan ia rasa macam bloated dan tak standard. Dengan v30 ini, data dapat disimpan dalam tempat khas. Nodes juga boleh pangkas (prune) metadata ini tanpa sebarang kerumitan untuk meringankan storan. Dalam POV pelombong block, semuanya tentang insentif. Kalau data boleh membayar bil (melalui fees) elektrik, pelombong terima saja. 

## Bitcoin Knots

Knots adalah software alternatif kepada Bitcoin Core tadi. Dengan Knots, syarat 80 bytes tadi masih ditetapkan, cuma ia menambah fitur seperti penapisan untuk menolak transaksi yang dikesan sebagai spam. Di dalam camp Knots, OP_RETURN yang besar, tidak dibenarkan sama sekali. Inscription? Di blok dalam node level. Bayangkan ia seperti seorang sekuriti di sebuah parti: "Aliran tunai sahaja, data-data aneh tak boleh masuk".

Pembangun Knots percaya bahawa, data arbitrari adalah sebuah pembaziran ruangan block, menaikkan fees untuk pengguna biasa dan boleh mengundang lebih pengawalseliaan kalau ada nodes yang broadcast CSAM. Anggap lah Knots ni macam sebuah gerakan memberontak pengubahan ethos Bitcoin sebagai peer-to-peer electronic cash system dan bukan nya sebuah rangkaian untuk menyimpan gambar.

|        | v30                                                                | Knots                                                                                           |
|--------|--------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| Polisi | Permissive                                                         | Restrictive                                                                                     |
| Pros   | Efisien: mudah di parse, bersih dari cara "hack"                   | Kesucian terpelihara: menghormati visi Satoshi                                                  |
|        | Merangsang inovasi                                                 | Pengoperasi node bebas mengubahsuai filter mengikut kesesuaian prinsip                          |
|        | Lebih decentralized                                                | Fee lebih murah                                                                                 |
| Cons   | Banjir spam: fee jadi mahal bila rangkaian sesak                   | Kebimbangan pemecahan: Membelahkan rangkaian, mengelirukan pemula dan memperlahankan penggunaan |
|        | Alasan untuk kerajaan kawalselia bila ada transaksi "non-kewangan" | Kekejangan inovasi: Menghalang penggunaan yang sah                                              |
|        | Node jadi kembung                                                  |                                                                                                 |



## Kenapa peduli?

Wacana seperti ini tak lain dan tak bukan hanya sebuah drama dan politik dalaman pembangun Bitcoin. Untuk orang yang cuma pegang Bitcoin dalam wallet tak perlu peduli pun sebenarnya, tapi ia sedikit sebanyak mempengaruhi juga dari segi fees.

Tapi kalau kita zoom out situasi ini, pengoperasi node kena pilih sisi samada nak guna v30 atau Knots. Fees boleh naik kalau ada data hogging. Ia pernah terjadi pada tahun 2023 dulu sewaktu Bitcoin NFT popular yang dinamakan Ordinals. Fee macam tu baru sekecil kacang, v30 kalau diaktifkan boleh meningkatkan lagi nilai fee tersebut.

Kalau kita zoom out lagi melihat gambaran yang lebih besar: ini adalah sebuah ujian kepada jiwa sebenar Bitcoin. Adakah wajar ia berkembang seperti teknologi blockchain lain (Ethereum, Solana) atau kekal menjadi sebuah rangkaian khas untuk hantar dan terima duit saja?

## 2 sats dari aku

Aku peduli pasal ni sebab sedekad sudah aku mengikut perkembangan Bitcoin. Aku lihat ia berkembang dari retail-driven sampai lah ke sekarang, institutional-driven. Selain itu, diskusi macam ni buatkan aku lebih menghargai bagaimana Bitcoin berkembang bukan saja sebagai satu sistem yang statik, tetapi ditambahbaik untuk menjadi lebih kuat.

Kalau aku, aku akan pilih Knots. Sebab aku lebih suka Bitcoin dengan cara yang lama, kekal tradisi sebagai sistem tunai p2p. Penambahbaikan yang perlu dibuat adalah dari segi privasi, sekuriti dan pengurusan fee. Kalau nak programmability atau smart contract, malah NFT, biarlah blockchain lain yang buat. Kerana lebih kompleks sebuah sistem itu, lebih banyak vektor serangan boleh di buat. Aku nak tengok Bitcoin berjaya tembus pasaran retail, di mana kita boleh belanja Bitcoin untuk beli barangan di kedai. Dah sedekah lama nya teknologi ini, tapi masih tak nampak poster "Bitcoin accepted here".

1: https://nbx.com/blog-en/bitcoin-bips-and-upgrades-what-you-need-to-know
2: https://www.bitstamp.net/en-gb/learn/crypto-101/exploring-bitcoin-forks/

[1]: https://luangdiri.github.io/2014/02/08/cryptocurrency-bitcoin.html
[2]: https://luangdiri.github.io/2025/06/23/bitcoin-100k-dan-seterusnya.html
[upgrade-timeline]: https://i.imgur.com/ee0eyce.png
[3]: https://www.reddit.com/r/btc/comments/14dhy5g/main_consensus_forks_of_bitcoin_june_2023_update/