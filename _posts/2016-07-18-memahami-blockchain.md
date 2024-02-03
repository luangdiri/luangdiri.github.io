---
title: "Memahami Blockchain"
date: 2016-07-18T00:00
tags:
- blockchain
---

<img alt="Blockchain logo" width="700" src="https://i.imgur.com/S5um2JY.png"/>

Beberapa tahun selepas Bitcoin menjadi cryptocurrency yang diguna orang ramai, teknologi blockchain menjadi satu topik yang hangat dikaji oleh para penyelidik. Blockchain terdapat pelbagai kegunaan antara nya pendaftaran nama dan smart contracts. Kaedah konsensus seperti proof-of-work (PoW) dan proof-of-stake (PoS) juga ada diterapkan dalam teknologi ini bagi membantu prinsip decentralization berjalan dengan lancar. Walau advanced macam mana pun, masih lagi terdapat lubang lubang sekuriti yang tidak di kenalpasti pada awal waktu ia dilahirkan.

Rupanya aku baru tahu, banyak jenis blockchain selain yang digunakan oleh Bitcoin. Maklumlah, code nya open source. Manusia pula kreatif nak tukar tukar ikut sukahati. Maka evolusi blockchain ni jadi makin bertambah.

So, dalam entri ni aku nak belajar pasal beberapa jenis blockchain yang digunapakai oleh platform seperti Bitcoin, Ethereum, Peercoin, Namecoin, New Economy Movement (NEM) dan juga Ripple. Sebab aku curious, that's why. Dalam Ethereum, aku berminat nak study pasal smart contract nya yang di war war kan bagus untuk prinsip decentralized platform. Dalam Namecoin, fokus nya adalah dalam bidang pendaftaran nama atau nama glamor nya, name registration. Peercoin pula, kaedah PoS nya; Ethereum juga _akan_ menggunakan [kaedah yang sama](http://ethereum.stackexchange.com/a/1927/1053). Platform yang lain tu aku bincangkan dekat bawah. It's gonna be long kot. Tapi aku akan keep it non-technical sebab aku pun bukan faham sangat secara technical benda alah ni.

Sebelum ni pun aku ada tulis pasal [Bitcoin][5] dan juga satu lagi post pasal benda yang [sama][6]. Tapi macam tak cukup lah dua post tu, simple sangat.

---

_**UPDATE:**_ Disebabkan satu topik je dah panjang lebar, aku pecah pecah kan lah. Mudah aku nak merepek dalam tu. *(Update: Aku malas menulis, jadi artikel-artikel yang tiada link tu aku tak buat)*

1. **Modern Blockchain**
2. Ethereum
3. Peercoin
4. Namecoin
5. New Economy Movement
6. Ripple
7. [[Appendix I] Terminology](http://blog.xxx.com/2016/09/24/appendix-i-terminology/)
8. [[Appendix II] Consensus algorithm][99]

---

## Latar belakang

Apa itu Blockchain? Blockchain ni adalah satu buku rekod awam digital yang hidup di internet. Kalau tak percaya, boleh tengok kat [sini](http://blockchain.info). Secara kasarnya, cuba bayangkan buku accounting yang penuh dengan transaksi sesebuah syarikat, di mana terdapat nya dari mana duit tu datang, dan ke mana duit tu pergi. Begitu juga blockchain. Ia menyimpan segala data transaksi user dari awal sampai... kiamat kot (inshallah).

Secara konvensional, database digunakan sebagai tempat pertengahan untuk menyimpan data data transaksi dalam sesebuah organisasi. Segala proses komputasi dilakukan disini. Juga kawalan pusat data ni juga dikawal oleh satu entiti, di mana ia mengurus, meng-update, transparency pun kelaut, dan orang luar nak pastikan data tu tak di manipulate pun susah <sup>22</sup>. Teknologi Blockchain dapat menyelesaikan masalah masalah centralization seperti ini.

Perlu di ingatkan juga, blockchain bukan sahaja mampu menyimpan data kewangan, tetapi juga data data lain seperti kontrak peguam, surat cinta, or even vote rakyat Malaysia untuk PRU akan datang. You name it, sumbat aje dalam block tu.

## Jadi, apa baik nya transparent ledger ni?

_"Bukan ke kalau lagi transparent data kau, lagi bahaya di buat nya?"_, soalan ni selalu masuk dalam telinga aku bila aku explain pasal Blockchain dekat member member. Ada kebaikan kebaikan pakai benda alah ni. Bukan nak kata cara standard sekarang dah rosak, bukan. Cuma dengan menggunakan model teknologi ni, kita akan dapat perspektif baru dalam bisnes model yang akan diterapkan dalam sesebuah organisasi. Point point dibawah diambil daripada [Goldman Sachs][26] (GS) research bertarikh 24 May 2016 ni, menyatakan:-

#### 1. Kemudahan yang selamat, transaksi yang tiada pusat (decentralized)

GS menyatakan bahawa dengan menggunakan blockchain model, salah satu kegunaan yang praktikal ialah dalam bidang Internet of Things. Ni satu contoh lah. Contoh yang diberi ialah **Smart Grid** (ie. solar panels), di mana secara analogi nya, TNB Malaysia di-decentralized kan model pengedaran tenaga nya kepada pengguna. Juga disebabkan nature of transparency ni, bill pembayaran juga dapat di transparent kan agar mudah consumer nak tahu berapa banyak tenaga yang di consume.

Aku rasa nak di apply kan dekat Syabas pun ok kot. At least dengan incorporate IoT dalam rumah (smart home) + sharing economy (sistem perkongsian bekalan air) + Blockchain, senang nak track berapa banyak air yang diguna, do the math, maka dapatlah bill pembayaran yang setimpal dan saksama.

#### 2. Mengurangkan penipuan (corruption) dan meningkatkan trust dengan security yang kuat

Ni takpayah cakap lah. Budak 3 tahun yang tinggal kat Malaysia pun boleh faham kot dengan keadaan politik Malaysia sekarang. Corruption everywhere. Bayangkan kalau Najib guna bitcoin untuk terima dana dari kerajaan Arab 2.6 billion tu, mudah je #TheRakyats nak sahihkan info ni dalam Blockchain. Huhu.

Anyway, dalam kes ni, GS ada buat case study pasal Airbnb jika ia menerapkan Blockchain dalam model bisnes nya, especially dalam reputation sistem. So apa baik nya? Setiap transaksi di encode dengan cryptography yang jitu dan transaksi ni di validate kan oleh network nodes (P2P). Jadi segala percubaan untuk mengubah atau membuang data akan terkantoi lah. Sangat berguna kalau nak di integrate dengan apa apa sistem reputasi.

#### 3. Meningkatkan transparensi dalam transaksi lebih dua orang

Benda macam ni lah buat **Smart Contracts** berguna. Dalam apa apa transaksi yang melibatkan lebih dua parti, kadang kadang sorang tu mesti nak percent lebih. Mana boleh. Kapitalism segera dapat di tamatkan kalau pakai Blockchain. Smart Contract ni kira dapat nak distribute funds antara entiti yang terlibat, dan ia transparent! Proses jadi lebih efisien dan dapat mengurangkan kos penghantaran dana kepada parti parti terlibat.

## Bagaimana Blockchain berfungsi

Semua transaksi yang telah berjaya akan dimasukkan dalam blockchain dan dengan cara ini, wallet Bitcoin akan dapat kira berapa banyak balance yang boleh dibelanjakan dan setiap transaksi baru hendaklah disahkan oleh pemegang sebenar wallet tersebut. Keutuhan dan susunan kronologi Blockchain juga diperketatkan dengan pelbagai algorithm cryptography <sup>6</sup>.

Transaksi ialah satu penghantaran nilai matawang Bitcoin yang akan dimasukkan ke dalam Blockchain. Bitcoin wallet pula menyimpan data pemegang wallet yang dipanggil private key atau seed, yang juga digunakan untuk sign transaksi dengan menggabungkan kaedah matematik untuk mengesahkan yang transaksi tersebut datangnya dari pemegang wallet tersebut. Tujuan sign tersebut juga dapat mengelakkan sesebuah transaksi daripada diubah oleh sesiapa selain pemegang wallet. Satu proses yang dipanggil **mining** pula akan dijalankan sejurus selepas transaksi dilakukan untuk mengesahkan transaksi tersebut <sup>6</sup>. Dengan bantuan P2P network, transaksi transaksi ini akan di broadcast kan ke seluruh network Blockchain. Jadi semua orang akan dapat ledger yang sama.

Mining, seperti diperkatakan tadi, adalah satu sistem algorithm yang digunakan untuk mengesahkan transaksi dengan memasukkan ia kedalam Blockchain. Blockchain menyusun setiap transaksi dalam susunan kronologi bagi melindungi network daripada ketidak-saksamaan <sup>6</sup>. Untuk sesebuah transaksi disahkan secara halal, ia haruslah di pack kan ke dalam satu block yang seterusnya akan di verify oleh network (miners) yang dikuatkuasakan oleh cryptography. Macam yang aku sebut tadi, susunan kronologi ni penting so that setiap block yang sebelumnya tidak dapat di ubah, sebab kalau ia di ubah maka ia akan membuatkan semua block jadi tak sah. Jatuh hukum haram di sisi undang undang Satoshi Nakamoto. Para miners pula yang bekerja keras untuk mengesahkan block block ni akan di beri upah dalam BTC.

So, all in all, transaksi BTC dalam Blockchain network ni boleh diterangkan di dalam satu infographic [ini](http://imgur.com/l0tAS4n).

## Implementasi

- Cryptocurrency -- eg. Bitcoin, Litecoin, Dogecoin, Trumpcoin
- Blockchain platform -- eg. Ethereum (from-scratch platform untuk smart contract)
- Sidechains -- eg. Rootstock (provide smart contract untuk Bitcoin)
- Distributed ledgers/private blockchains -- eg. R3 untuk industry kewangan

## Market capitalization

Market capitalization atau melayu nya permodalan pasaran, adalah satu penunjuk tentang penguasaan sesuatu Blockchain dalam trading market. Bitcoin, dalam hal ini, adalah pemain paling besar yang telah menjangkaui ~10 billion USD market cap. Diikuti dengan Ether, yang hampir mencecah 1 billion USD <sup>16</sup>. Nak tengok market cap untuk lain lain cryptocurrency boleh tengok kat [sini][22].

## Scripting language, Turing completeness and smart contracts

Kebanyakan cryptocurrency punyai scripting language mereka tersendiri yang boleh di tempel ke dalam mana mana transaction. Sesetengah daripada nya adalah **Turing complete**; [Solidity][33] language dalam Ethereum. Benda ni penting untuk run Smart Contract yakni sebuah program untuk menjalankan contract antara pengguna menggunakan consensus <sup>16</sup>.

Sebagai contoh, smart contract amat berguna untuk company insuran untuk membayar ganti rugi secara _automatik_ jika berlaku apa apa kemalangan etc. Banyak lagi kegunaan smart contract dalam dunia ni, to put it in another perspective, something yang melibatkan pembayaran secara berkala. Contoh islamik sikit, ambil lah pembayaran zakat fitrah atau pendapatan. Tapi takleh gak kot, sebab zakat perlukan akad hehe. Cukai pendapatan, cukai pintu, bayar road tax etc. 

Contoh yang lagi kompleks dalam penggunaan kontrak pintar ni ialah [DAO][27] (Decentralized Autonomous Organization), yakni sebuah syarikat yang takde manager atau pekerja office. Semua dijalankan oleh smart contract (code). Yeah man, in code we trust <sup>16</sup>.

## Isu isu berkaitan

Antara isu kontroversi yang aku dapat korek dari [internet](https://www.oreilly.com/ideas/blockchain-scalability) ialah:

#### 1. Centralization

Kecenderungan terhadap centralization dengan Blockchain yang membesar; dengan meningkatnya tumbesaran Blockchain, lagi banyak keperluan storage, bandwidth dan computational power yang diperlukan oleh "full nodes" dalam network, menjuruskan lebih banyak risiko ke arah centralization kalau Blockchain makin besar, yang hanya beberapa nodes sahaja yang boleh proses sesebuah block. Topik ni hangat diperkatakan dalam isu 51% attack yang akan aku huraikan tak lama lagi kat bawah.

#### 2. Block size limit

Isu yang berkaitan dengan miners dan Bitcoin adalah daripada built-in hardcoded dalam Bitcoin Core terhad kepada 1 MB per block (~10 minutes) untuk block disahkan. Limit ni tak boleh di ubah (nak increase or decrease) tanpa [hard-fork](https://en.bitcoin.it/wiki/Hardfork); yakni topik yang _agak_ controversial dalam mana mana decentralized systems.

Kenapa 1 MB je, kau tanya. Sebab nak prevent dari DoS attack, kata sorang mamat ni dari [bitcointalk][3] forum. Boleh juga baca [sejarah][2] pasal block size ni.

#### 3. Fee transaksi yang kian meningkat

Fee yang tinggi untuk transaksi Bitcoin, dan potensi untuk fee tersebut di tingkatkan bila mana network membesar. 

## Attack vectors

> _"Every great system, has its greatest flaws"_  
> &mdash; John Doe

Aku lupa mana aku baca quote kat atas ni. Yang aku ingat ia datang dari some ethical hacking ebook yang aku baca /shrug. Kita tahu Blockchain/Bitcoin ni adalah satu inovasi yang mantap lagi powderful. Tapi malangnya quote tadi tu memang apply dekat apa apa teknologi. No matter how powerful your system is, mesti ada masalah yang tak dapat dielakkan.

Antara isu sekuriti yang mengelilingi teknologi Blockchain seperti:-

#### 1. 51% attack

Ia mungkin terjadi (secara hipotetikal) apabila pemegang cryptocurrency memiliki 51% daripada semua share dalam sesebuah network. Dalam kes bitcoin, miners yang memegang 51% daripada pool network mungkin berkuasa untuk menyalahgunakan kuasa untuk buat consensus yang berat sebelah untuk lakukan double spending. Mereka ada kuasa penuh dalam network dan mampu memanipulasi rekod awam (Blockchain) dengan sukahati <sup>18</sup> <sup>19</sup>. Kalau terjadi lah, maka konsep demokrasi akan berubah menjadi diktatorship :(
    
**Well, not quite**

Kata website [ni][24], ada beberapa benda aje manusia yang ada 51% pegangan ni boleh buat. Mereka mampu menghalang transaksi daripada apa apa confirmation, maka membuatkan ia invalid, secara potensi nya menghalang orang daripada membuat transaksi antara address. Mereka juga mampu me-reverse transaction yang mereka hantar sewaktu mereka dalam kuasa (membolehkan double spending terjadi), also mampu menghalang miners daripada menjumpai block. Website tu pun cakap, mereka takleh pun nak reverse transaction yang dah jauh melampau, buat coin baru (selain mining), atau curi coin daripada wallet orang.

**Kenapa 51% je?**

Sebab 51% kira dah lebih setengah daripada keseluruhan penggunaan network.

**Bolehkah ia terjadi pada PoW atau PoS?**

Dua dua.

Ikut pengetahuan aku, kalau dalam **PoW** (melibatkan mining), pool mana yang ada hash rate >50% tu lah yang akan trigger benda ni. Dalam hal ni, mining pool yang popular ada potensi dalam attack ni berbanding dengan orang yang mine sorang sorang. Tapi boleh jadi juga orang tu ada supercomputer di mana hashing power dia over 9000.

Ni aku tak pasti tapi kalau salah tolong betulkan; dalam **PoS** pula, orang yang ada pegangan >50% daripada total existence sesuatu coin mampu lakukan attack ni. Tapi tak sewenangnya kot. Mesti ada some mechanism dalam PoS scheme yang boleh halang 51% attack terjadi. Tak jumpa pulak source dari interwebs.

#### 2. Sybil attack

Nama Sybil ni diambil daripada sebuah buku yang ditulis oleh Flora Rheta Schreiber pasal perempuan yang ada dissociative identity disorder <sup>23</sup> (multiple personality disorder). "So, apa kaitannya?", jiran kau tanya. Well, serangan Sybil ni berlaku apabila seseorang tu merosakkan sesebuah sistem reputasi dalam P2P network dengan membuat beribu lemon fake identity dan menggunakannya untuk mendapat pengaruh dalam network tu <sup>23</sup>.

Serangan ni dah dijumpai sejak zaman BitTorrent lagi, sejak P2P network mula berkembang. Menurut Douceur dalam paper dia ["The Sybil Attack"][29], kalau seorang entiti dapat melambangkan banyak entiti entiti lain, sebuah sistem boleh dikawal oleh nya, maka dapat melemahkan lagi sebuah sistem. Sebab P2P network di reka untuk percaya kepada kepercayaan (trust) antara node lain. Benda ni tak mustahil boleh terjadi.

Satu contoh mudah yang tak melibatkan P2P network adalah bila kau buat banyak account Gmail semata mata nak vote kawan kau dalam internet poll. Itu lah dikatakan dengan sistem reputasi yang ada lobang. Selain itu, company juga boleh guna attack ni untuk dapat kan rating Google Page Rank yang baik <sup>22</sup>.

Beliau berkata lagi, satu jalan penyelesaian untuk mengelakkan benda ni terjadi adalah dengan adanya 3rd party agensi untuk mengesahkan identiti. Sebagai contoh, [VeriSign][31].

**Apa attacker boleh buat dalam kes Blockchain?**

Dalam konteks voting system, sebuah kerajaan mungkin boleh koordinat serangan ni untuk menang dalam election (katakan lah voting dibuat dalam Blockchain). Yes, voting guna Blockchain is already a work in progress; [BitCongress][34].

Tapi dalam kes ni, serangan ni susah untuk dilakukan sebab penggunaan consensus PoW [(more info)][32]. Secara ringkasnya, node akan percaya kepada chain yang paling banyak proof of work. Kalau node tu cuba untuk jadi Sybil, tapi takde proof of work (tak mine block), maka ia adalah fake.

#### 3. Double spending

Oh, rupanya blockchain.info ada buat page untuk [double spent transaction][38]. Interesting.

So, apa itu double spending? Menurut Bitcoin stackexchange [accepted answer][39] ni, ia adalah satu attack di mana satu coin di belanja dalam lebih dari satu transaction. Katanya lagi ada [beberapa cara][40] nak melakukan perbelanjaan dubel:

1. **Race attack**

Hantar dua transaction yang bercanggah dalam masa yang singkat. Kira macam tekan button send serentak la kot.

**Prevention**  
Tunggu 1 confirmation dikeluarkan dalam sesuatu transaction. Baru lah confirm ia betul.

2. **Finney attack**

Memerlukan miner (attacker) dalam serangan ni.  
Merchant side (recipient) membenarkan unconfirmed transaction.  
Di mana attacker ni perlu pre-mine (mine normally tapi tak broadcast ke network) satu block yang mengandungi coin dia sendiri (hantar ke diri sendiri) dan belanja coin yang sama kepada merchant yang accept unconfirmed transaction tadi. Lepas tu baru dia release kan block tersebut untuk dapatkan balik coin dia tadi tu. Maksudnya dia hantar ke merchant -- which dapat angin je -- dan juga dapat balik coin yang di pre-mine tadi.

**Prevention**  
Tunggu at least 6 confirmation, baru confirm ia betul... Wait, that doesn't makes sense. Maybe kena baca apa yang mamat ni tulis dalam [master thesis][42] dia.

More on [Finney attack][41] explanation.

3. **51% attack**

Dah explain kat atas sana, sila scroll. Basically, memiliki 51% of total computing power (ie. PoW scheme), dan mampu me-reverse apa apa transaction yang dia nak. Like a boss.

**Prevention**  
None lol jk. Google sendiri and deal with it :p

#### 5. Security vulnerabilities & bugs

Benda ni macam simple. Ye lah, sebagai programmer hari hari aku jumpa kumbang, lipas, cicak dalam code aku. Tapi dalam kes blockchain ni macam interesting pulak aku tengok, sebab sekali ada bug yang critical, kena hard-fork seluruh network. Lepas tu kena suruh semua client dan miners untuk upgrade software dia. Tak ke leceh gitu?

Contoh paling dekat ialah [DAO hack][44] haritu lah. Hambik kau, tengah best layan harga Eth nak sampai 25 USD sekali kena hack (tak hack lah, cuma ada flaw dalam smart contract code). Merudum jatuh sampai ke [10 USD][45]. Menangis wei. Sekarang ni sibuk lah diskus kat Reddit bagai nak hardfork ke nak softfork. Tapi baru baru ni, mereka dah capai consensus dah pun Ethereum blockchain akan di [hardfork][46] Rabu ni (20 July 2016) @ block 1920000. Jadi kita tengok lah ramai ramai apa terjadi.

Anyway back to the topic, kat [sini][47] kita boleh tengok senarai bugs dan vulnerabilities, dan juga fixes nya.

## Scalability

Part ni aku cuma nak faham apakah yang dipersoalkan dengan "blockchain scalability" oleh segelintir manusia yang skeptik (developer) di ruang internet. Adakah dengan merekod segala transaksi Bitcoin dalam Blockchain dapat menjejaskan sekuriti atau kebolehan nya menangani amount transaksi yang besar?

Dalam perkara ni, Bitcoin cuba nak compete dengan payment gateway yang dah mainstream seperti Visa dan Paypal. Visa dapat handle dalam 2000-4000 transaksi sesaat (tps), secara puratanya. Paypal pula dapat handle dalam 10 juta transaksi sehari atau 115 tps. Bitcoin on the other hand, terhad kepada 7 tps disebabkan protokolnya hanya menyokong block size sehingga 1 MB <sup>11</sup>.

_Note: Aku korek info bawah ni dari [Bitcoin wiki][4], tapi dia kata benda ni dah outdated. But I'm gonna write it anyway._

Selain daripada tu, CPU, storage dan network utilization juga menjadi satu masalah scalability dalam Bitcoin network. Basically, wiki tu nak kata secara teori nya nak capai 4000 tps, CPU, storage, dan network kena lah power. Tapi Bitcoin punya current build belum boleh capai sehebat Paypal atau Visa. So, author tu pun ada juga menceritakan pasal optimization yang boleh diterapkan untuk membuat Bitcoin ni lebih scalable dari segi hardware utilization ni, seperti CPU dan Simplified payment verification (SPV).

Atas sebab nature Blockchain/Bitcoin ni adalah sumber terbuka, ada juga cadangan cadangan penambahbaikan platform ni daripada komuniti seperti:-

#### 1. [Ultimate blockchain compression][7]

Satu idea meng-compress blockchain untuk mencapai "trust-free lite nodes" (lol wtf is that?). Seperti yang sudah di tl;dr kan oleh seorang Reddit user [ripper2345][10], ia adalah satu cadangan untuk mengasingkan blockchain -- bukan fork, tapi miners masih mine di chain yang sama -- yang lagi ringan daripada chain utama. Chain yang terasing ni hanya menyimpan "[unspent transaction outputs][11]" sahaja. Ia bagi mendapatkan muat turun Blockchain yang pantas untuk client yang tak mine, tanpa memerlukan trust untuk 3rd party.

#### 2. [The Mini-Blockchain Scheme][8]

Kalau nak tahu, Blockchain sekarang ni agak "bulky" dengan benda benda sampah (although data yang berguna), tapi benda benda ni semua buat Blockchain kita ni berat. So scalability issue pun datang. Dalam proposal ni mencadangkan, transaksi yang dah lama boleh di lupa kan oleh network. Since kita tahu sesebuah node hanya pakai bahagian chain yang baru untuk sync kita panggil bahagian ni **"[mini-blockchain][13]"** (bahagian 1). Proposal ni pun dah kenalpasti bahawa sekuriti akan terjejas kalau buat macam ni, tapi ia boleh di selesaikan dengan **"[proof chain][14]"** (bahagian 2) dan data data ownership coin yang dah dilupakan tu boleh diselesaikan dengan membina satu database yang memegang address yang ada isi dalam nya, dipanggil **"[account tree][15]"** (bahagian 3).

So basically, benda ni ada 3 bahagian, mini-blockchain, proof chain, dan account tree. Katanya lagi, Proof chain ni mengawal mini-blockchain, dan mini-blockchain ni mengawal account tree. Antara benefit yang ada dalam proposal ni adalah ia dapat mengetatkan lagi keutuhan dan sekuriti, transaksi yang pantas dan dapat menurunkan fee, network sync yang laju, dapat menyokong trafik yang tinggi, ruang block yang lagi besar untuk custom messages dan ada potensi untuk tingkatan anonymity.

#### 3. [Lightning network][9]

Dicadangkan oleh Joseph Poon dan rakan sekerja nya Thaddeus Dryja, Lightning Network bermaksud Rangkaian Kilat lol. Budak berdua ni cadangkan, macam mana satu layer atas Bitcoin blockchain dapat membolehkan transaksi yang murah dan segera, sambil meningkatkan lagi scalability. Boom! Simple je idea dia.

Aku pernah dengar benda ni dekat Ethereum, tapi taktau pulak ia bermula dari Bitcoin. Rupanya baru aku tahu, ia hanya lah satu teori; takde code whatsoever, tapi still a work in progress <sup>12</sup>. Disebabkan tu, ramai dari kalangan komuniti buat [argument][16] dan tohmahan tak berfakta terhadap teori ni berkenaan dengan campur tangan Blockstream -- sebuah company yang membina aplikasi Bitcoin specifically dalam bidang sidechain.

**Macam mana ia berfungsi?**

Secara am nya, Lightning Network di reka untuk mengurangkan volume dalam Blockchain dengan menyalurkan transaksi dan [mikro-transaksi][20] (transaksi kurang dari 1 satoshi dan biasanya wujud dalam fee penghantaran) kepada satu sidechain lain, yang juga sync dengan Blockchain utama, dalam jarak waktu yang tetap.

**Kenapa?**

Atas sebab isu scalability, Bitcoin sekarang ni hanya menyokong ~7 tps dan limited kepada 1 MB ruang block. Untuk meluaskan lagi kuasa nya untuk mencapai 45000 tps, proposal ni cakap transaksi Bitcoin haruslah di lakukan di luar Blockchain <sup>18</sup>.

Baca selanjutnya di [sini][18] dan summarized paper [sini][19]. Also, berita pasal [alpha release][21] of Lightning Network dalam Blockchain.

Tapi benda benda kat atas ni pun macam proposal yang 2-3 tahun lepas, yang dah lama. Aku sure kat luar sana banyak lagi proposal untuk menambahbaik scalability teknologi ni.

## Use cases

Basically, this [infographic][35] tells everything pasal use cases of Blockchain. Yang [ni][36] pun interesting juga. But, to put it simply, beberapa general use-cases such as:-

1. Smart contracts
2. Smart assets
3. Clearing and settlements
4. Payments
5. Digital identity

[More info][37] <small>(sebab dah malas nak tulis)</small>

## Perbezaan antara aplikasi Blockchain

Di sini kita boleh lihat matrix perbezaan antara teknologi Blockchain yang digunapakai oleh aplikasi lain.

Kredit kepada **coinfabrik.com** for making this [chart][23].

---

#### Resource

1. https://www.oreilly.com/ideas/understanding-the-blockchain
2. New kids on the block: an analysis of modern blockchains - http://arxiv.org/pdf/1606.06530v1.pdf
3. https://en.bitcoin.it/wiki/Double-spending
4. http://ethdocs.org/en/latest/introduction/what-is-ethereum.html
5. https://en.wikipedia.org/wiki/Ethereum
6. https://bitcoin.org/en/how-it-works
7. https://en.bitcoin.it/wiki/Scalability_FAQ#What_is_the_short_history_of_the_block_size_limit.3F
8. https://en.bitcoin.it/wiki/Block_size_limit_controversy
9. https://en.bitcoin.it/wiki/Weaknesses
10. https://en.bitcoin.it/wiki/Full_node
11. https://en.bitcoin.it/wiki/Scalability
12. https://bitcoinmagazine.com/articles/debunking-the-most-stubborn-lightning-network-myths-1444837807
13. http://www.coindesk.com/could-the-bitcoin-lightning-network-solve-blockchain-scalability/
14. https://lightning.network/lightning-network-summary.pdf
15. http://cointelegraph.com/news/the-crack-of-thunder-blockchaincom-announces-alpha-release-of-lightning-network
16. http://blog.coinfabrik.com/overview-of-blockchain-technologies/
17. https://www.evry.com/globalassets/insight/bank2020/bank-2020---blockchain-powering-the-internet-of-value---whitepaper.pdf
18. https://www.barclayscorporate.com/content/dam/corppublic/corporate/Documents/insight/blockchain_understanding_the_potential.pdf
19. https://learncryptography.com/cryptocurrency/51-attack
20. https://en.bitcoin.it/wiki/Proof_of_Stake#Infeasability_of_standard_attack_vectors
21. http://www.the-blockchain.com/docs/Goldman-Sachs-report-Blockchain-Putting-Theory-into-Practice.pdf
22. http://anti-virus-software-review.toptenreviews.com/what-is-a-sybil-attack-.html
23. https://en.wikipedia.org/wiki/Sybil_attack
24. https://letstalkbitcoin.com/blog/post/solution-to-sybil-attacks-and-51-attacks-in-decentralized-networks

[1]: http://arxiv.org/pdf/1606.06530v1.pdf
[2]: https://en.bitcoin.it/wiki/Scalability_FAQ#What_is_the_short_history_of_the_block_size_limit.3F
[3]: https://bitcointalk.org/index.php?topic=608561.msg6723793#msg6723793
[4]: https://en.bitcoin.it/wiki/Scalability
[5]: http://blog.xxx.com/2015/01/29/bermula-dengan-bitcoin/
[6]: http://blog.xxx.com/2015/01/28/cryptocurrency-bitcoin/
[7]: https://bitcointalk.org/index.php?topic=88208.0
[8]: http://cryptonite.info/files/mbc-scheme-rev2.pdf
[9]: https://lightning.network/lightning-network-paper.pdf
[10]: https://www.reddit.com/r/Bitcoin/comments/xanm2/ultimate_blockchain_compression_w_trustfree_lite/
[11]: http://bitcoin.stackexchange.com/questions/4301/what-is-an-unspent-output/4302#4302
[13]: http://cryptonite.info/wiki/index.php?title=Mini-blockchain
[14]: http://cryptonite.info/wiki/index.php?title=Proof_chain
[15]: http://cryptonite.info/wiki/index.php?title=Account_tree
[16]: https://bitcoinmagazine.com/articles/debunking-the-most-stubborn-lightning-network-myths-1444837807
[17]: https://blockstream.com/
[18]: http://www.coindesk.com/could-the-bitcoin-lightning-network-solve-blockchain-scalability/
[19]: https://lightning.network/lightning-network-summary.pdf
[20]: https://blog.coinbase.com/2013/08/06/you-can-now-send-micro-transactions-with-zero-fees/
[21]: http://cointelegraph.com/news/the-crack-of-thunder-blockchaincom-announces-alpha-release-of-lightning-network
[22]: https://coinmarketcap.com/
[23]: http://blog.coinfabrik.com/wp-content/uploads/2016/07/Blockchain-Players-and-Feature-Matrix.pdf
[24]: https://learncryptography.com/cryptocurrency/51-attack
[25]: https://www.peercointalk.org/index.php?topic=244.0
[26]: http://www.the-blockchain.com/docs/Goldman-Sachs-report-Blockchain-Putting-Theory-into-Practice.pdf
[27]: http://daohub.org/
[28]: https://en.wikipedia.org/wiki/Sybil_attack
[29]: http://freehaven.net/anonbib/cache/sybil.pdf
[30]: http://anti-virus-software-review.toptenreviews.com/what-is-a-sybil-attack-.html
[31]: https://www.verisign.com/
[32]: http://bitcoin.stackexchange.com/questions/42177/can-we-make-sybil-attacks-virtually-impossible
[33]: https://solidity.readthedocs.io/en/latest/
[34]: http://bitcoininc.xyz/BitCongress_Whitepaper.pdf
[35]: https://ipfs.pics/ipfs/QmQfJirTD5cmmqtHbmJd8chXPrX72evJMPkHPdSS75CjyX
[36]: https://ipfs.pics/ipfs/QmdPybkHPrPQDnhVMq4cWuQzF6pYcpoT8MkGScv5kKM6Y4
[37]: http://www.finyear.com/The-five-major-use-cases-for-financial-blockchains_a35655.html
[38]: https://blockchain.info/double-spends
[39]: http://bitcoin.stackexchange.com/a/4976/15513
[40]: https://en.bitcoin.it/wiki/Double-spending
[41]: http://bitcoin.stackexchange.com/questions/4942/what-is-a-finney-attack
[42]: https://bitcointalk.org/index.php?topic=88149.0
[44]: https://blog.ethereum.org/2016/06/17/critical-update-re-dao-vulnerability/
[45]: https://ipfs.pics/ipfs/QmTG1ZXGfnLMBf9LnoReryhug2KA732gLXKBCUFvdDT6vT
[46]: https://www.reddit.com/r/ethereum/comments/4t9mzg/quick_hf_safety_tips_for_users/
[47]: https://en.bitcoin.it/wiki/Common_Vulnerabilities_and_Exposures
[99]: http://blog.xxx.com/2017/02/04/consensus-algorithm/
