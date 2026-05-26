# Indicator Toolkit

Folder `indicator/` sekarang jadi rumah utama script Pine di repo ini. Ini bukan folder strategy atau backtest. Semua file `.pine` di sini ditulis dengan `indicator(...)`, bukan `strategy(...)`.

Artinya:

- tidak ada order fill bawaan
- tidak ada entry or exit backtest
- tidak ada performance report strategy tester
- sinyal dipakai buat bantu baca chart, bukan buat hasil backtest otomatis

Folder `strategy/` sudah tidak dipakai untuk flow utama. Posisi script dipindah ke `indicator/` supaya konteksnya jelas dari awal, toolkit indikator TradingView Pine.

## Isi File

### `crypto_amt_session_bias_v1.pine`

Indikator session bias yang ringan untuk baca Asia, London, dan New York tanpa Volume Profile, VWAP, atau backtest.

- session detection pakai input Asia, London, New York, dan timezone `Etc/UTC` default
- kalau sesi overlap, prioritasnya tetap New York > London > Asia
- track current session open, high, low, mid, dan opening range
- simpan high/low session sebelumnya untuk deteksi sweep dan reclaim
- bias mulai netral tiap sesi, lalu berubah bullish setelah reclaim dari low sweep atau bearish setelah reclaim dari high sweep
- ada marker sweep, marker reclaim/bias, dashboard top-right, background tint ringan, dan alert TradingView

Pilih Session Bias v1 kalau targetmu cuma membaca alur session: apakah harga sweep high/low session sebelumnya lalu reclaim. Ini lebih sederhana daripada toolkit confluence v3/v6, dan cocok dipasang berdampingan dengan indikator lain.

### `crypto_amt_session_bias_v2_companion.pine`

Companion session bias generasi baru yang tetap berdiri sendiri sebagai `indicator(...)`, bukan `strategy(...)`, dan bukan bagian logic internal toolkit v6 atau v7.

- tetap track Asia, London, dan New York secara terpisah
- kalau sesi overlap, display aktif tetap pakai prioritas New York > London > Asia
- perbaikan utamanya ada di semantics overlap: previous completed session high/low hanya update saat raw session end asli terjadi pada `asiaEnded`, `londonEnded`, atau `newYorkEnded`, bukan saat active priority bergeser karena overlap
- default dashboard ada di `Bottom Right`
- default `Dashboard detail` adalah `Compact`
- alert default tetap close-confirm, jadi notifikasi menunggu candle close saat opsi confirm aktif
- alert sengaja dibatasi jadi empat jenis saja: `Sweep High`, `Sweep Low`, `Reclaim After Sweep`, dan `Bias Flip`

Pilih Session Bias v2 Companion kalau kamu sudah nyaman pakai toolkit modern v6.1, v6.2, v7.2, atau v7.3, lalu butuh panel session bias yang berdampingan tanpa ikut mengubah logic confluence toolkit. Kalau kamu cuma butuh versi ringan lama, v1 tetap ada dan tetap tidak berubah. Panduan cepatnya ada di [Panduan Pemula Crypto AMT Session Bias v2 Companion](docs/crypto-amt-session-bias-v2-companion-panduan-pemula.md).

### `crypto_amt_ict_smc_companion_v1.pine`

Companion ICT/SMC compact yang tetap berdiri sendiri sebagai `indicator(...)`, bukan `strategy(...)`, dan bukan bagian logic internal toolkit v6 atau v7.

- membaca premium/discount dari range swing/fallback dan menampilkan equilibrium opsional
- mendeteksi FVG tiga candle dengan filter displacement berbasis ATR dan CE midpoint opsional
- menandai IFVG saat FVG aktif ditembus close-confirm ke sisi sebaliknya
- membuat scored order block ringan dari opposite candle, displacement/FVG context, premium/discount alignment, close terhadap range mid, dan freshness
- CISD hanya display-only, tidak mengubah sinyal toolkit utama
- dashboard default ada di `Bottom Left` supaya tidak tabrakan dengan dashboard toolkit modern yang sering ada di kanan
- alert sengaja dibatasi jadi dua jenis saja: `Bullish Setup` dan `Bearish Setup`, dengan close-confirm gate

Pilih ICT SMC Companion v1 kalau kamu ingin konteks tambahan ala SMC seperti FVG, IFVG, premium/discount, scored OB, dan CISD, tapi tidak ingin mengubah toolkit utama v6.2 atau v7.3. File ini cocok dipasang berdampingan sebagai layer konteks, bukan pengganti confluence toolkit utama. Panduan cepatnya ada di [Panduan Pemula Crypto AMT ICT SMC Companion v1](docs/crypto-amt-ict-smc-companion-v1-panduan-pemula.md).

### `crypto_amt_toolkit_RECOMMENDED.pine`

File paling aman untuk pemula yang ingin langsung mulai tanpa bingung pilih versi.

Saat ini isinya sama dengan `crypto_amt_toolkit_v3_precision.pine`, yaitu versi stabil yang paling direkomendasikan buat pemakaian harian. File ini dibuat sebagai entrypoint supaya pemula tidak otomatis memilih v5 hanya karena nomor versinya paling baru.

### `crypto_amt_toolkit_v3_precision.pine`

Versi stabil dan paling direkomendasikan buat dipakai harian.

- fokus ke confluence yang lebih ketat
- ada EMA trend filter dan HTF bias
- ada no trade zone biar tidak terlalu sering paksa entry di tengah value
- cocok kalau mau versi yang paling aman buat base utama

Pilih v3 kalau kamu mau set yang lebih matang, lebih konsisten, dan tidak butuh eksperimen fitur baru.

### `crypto_amt_toolkit_v6_stable_alert.pine`

Versi stable-alert berbasis v5. Fokusnya menjaga decision layer tetap sama, tapi alert utama lebih aman untuk candle realtime.

- default `Confirm signals on bar close` menyala, jadi Confirmed Long, Confirmed Short, dan Invalidated menunggu candle close
- sinyal mentah intrabar ditahan sebagai `PENDING LONG` atau `PENDING SHORT` di dashboard sampai candle close
- Early Warning tetap setup warning awal dan tidak ikut close-confirm, tapi tidak muncul di bar yang sama dengan kandidat raw confirmed long/short
- default visual lebih ringan: rejection mark off, flow dot off, dan Clean mode on
- v5 lama tetap file eksperimen terpisah dan tidak diubah

Pilih v6 kalau kamu suka alur v5, tapi ingin alert yang lebih aman dari perubahan sinyal intrabar. Ini cocok untuk alert TradingView yang tidak mau terlalu cepat aktif sebelum candle selesai. Kalau kamu ingin membandingkan behavior mentah tanpa guard close-confirm baru, buka v5 sebagai pembanding eksperimen.

### `crypto_amt_toolkit_v6_1_conservative_risk.pine`

Versi eksperimen berbasis v6 stable-alert. Fokusnya mengetes filter sinyal dan risk helper yang lebih konservatif tanpa mengubah v6 lama.

- close-confirm, `PENDING LONG` / `PENDING SHORT`, dan empat alert utama tetap mengikuti v6
- `Scalp` memakai relative volume minimum yang lebih ketat sebelum raw confirmed signal lolos
- low-relvol block dibuat lebih ketat untuk scalp, tapi tetap tidak memblok hanya karena volume rendah saat harga dekat VAH, VAL, POC, atau VWAP
- `AWAY FROM LEVEL` tetap jadi guard utama; impulse hanya bisa mendapat pengecualian kalau candle sudah close, satu arah, volume cukup, score lebih kuat dari syarat minimum, HTF valid, dan tidak sedang middle value / low relvol / chop
- `Risk mode` default `Structure`, sehingga formula invalidation lama tetap dipakai; `Hybrid` dan `Tight` disediakan untuk mengetes invalidation/target helper yang lebih dekat ke harga

Pilih v6.1 kalau kamu ingin mengetes apakah setup 5m/15m jadi lebih tahan terhadap volume kering dan whipsaw tanpa kehilangan behavior alert aman dari v6.

### `crypto_amt_toolkit_v6_2_info_auto_perf.pine`

Versi eksperimen berbasis v6.1 conservative-risk. Fokusnya mempertahankan layar yang kaya informasi, tapi menambah mode performa otomatis supaya timeframe besar lebih ringan.

- default visual tetap informatif seperti v6.1: VP boxes, VWAP bands, EMA, labels, risk helper, bar color, level tags, dan dashboard tetap on
- `Performance mode` default `Auto`; Auto resolve ke `Fast` pada 1H ke atas, dan ke `Balanced` pada timeframe lebih kecil
- `Fast` tetap mematikan lower-timeframe delta berat dan mengurangi beban Volume Profile seperti mode performa sebelumnya
- dashboard bisa dipindah lewat `Dashboard position`
- `Signal Grade` dan `Volatility` hanya display-only; tidak mengubah confirmed signal, alert, cooldown, risk, target, atau no-trade logic

Pilih v6.2 kalau kamu sekarang nyaman dengan v6.1 karena informasinya lengkap, tapi ingin default yang lebih aman dari timeout pada 1H tanpa perlu manual ganti `Performance mode` ke `Fast`.

### `crypto_amt_toolkit_v7_clean_execution.pine`

Versi clean-execution berbasis v6 stable-alert. Fokusnya bukan mengubah decision layer utama, tapi merapikan default chart dan dashboard supaya keputusan akhir lebih cepat terbaca.

- default `Confirm signals on bar close` tetap menyala, jadi behavior close-confirm tetap sama seperti v6
- dashboard tetap on, dan `Dashboard detail` default-nya `Focus`
- mode `Focus` menaruh baris `FINAL` dan `WHY` di bagian paling atas sebelum detail `Close Confirm`, score, context, active, flow, dan raw trigger
- mode `Full` membuka lagi detail debug seperti score parts, setup, block reason, risk, target, LTF delta, delta/relvol, PA, dan mode
- default visual dibuat lebih ringan: VP boxes, VWAP bands, EMA trend filter, setup or confirmed labels, risk helper, bar color, level tags, rejection marks, dan flow dots default off
- Clean mode tetap on supaya chart historis tidak cepat penuh, tapi user masih bisa menyalakan toggle visual satu per satu kalau butuh audit lebih detail

Pilih v7 kalau kamu suka alur close-confirm v6, tapi ingin tampilan yang lebih bersih dan dashboard yang langsung menekankan keputusan akhir serta alasan di baliknya. Ini cocok buat pemakaian harian saat kamu lebih sering butuh ringkasan eksekusi daripada panel debug penuh.

### `crypto_amt_toolkit_v7_1_conservative_clean.pine`

Versi eksperimen yang menggabungkan logic conservative-risk v6.1 dengan tampilan bersih v7.

- close-confirm, pending state, dan alert shape tetap sama seperti v6/v7
- logic volume, low-relvol, away-from-level exception, dan `Risk mode` mengikuti v6.1
- default chart tetap bersih seperti v7: VP boxes, VWAP bands, EMA, labels, risk helper, bar color, level tags, rejection marks, dan flow dots default off
- dashboard `Focus` tetap menaruh `FINAL` dan `WHY` di atas, sedangkan `Full` membuka detail debug termasuk mode dan risk mode

Pilih v7.1 kalau kamu ingin langsung mencoba versi conservative-risk dengan dashboard yang paling enak dibaca saat memantau chart.

### `crypto_amt_toolkit_v7_2_spec_watch.pine`

Versi eksperimen berbasis v7.1 conservative-clean. Fokusnya menambah pemantauan kandidat realtime yang sengaja default-off.

- `Enable Spec Watch (realtime only)` default off dan hanya bekerja saat `Confirm signals on bar close` menyala
- status `SPEC WATCH LONG` / `SPEC WATCH SHORT` hanya muncul di realtime bar yang belum confirmed, memakai gate setup, trigger, level, quality, scalp relvol, cooldown, dan no-trade yang sama
- Spec Watch tidak mengubah confirmed alert, active plan, invalidation, target, atau cooldown
- `Tint Spec Watch bars amber` default off; kalau dinyalakan, bar realtime kandidat diberi tint amber ringan
- tidak ada alertcondition Spec Watch karena kandidat realtime bisa berubah sebelum candle close

Pilih v7.2 kalau kamu ingin melihat calon sinyal realtime sebelum candle close, sambil tetap menunggu Confirmed Long/Short resmi setelah close.

### `crypto_amt_toolkit_v7_3_auto_performance.pine`

Versi eksperimen berbasis v7.2 spec-watch. Fokusnya menjaga tampilan clean v7.2, sambil menambah default performa otomatis dan beberapa info ringkas di dashboard.

- `Performance mode` default `Auto`; Auto resolve ke `Fast` pada 1H ke atas, dan ke `Balanced` pada timeframe lebih kecil
- behavior Spec Watch tetap sama seperti v7.2: default off, realtime-only, tanpa alert Spec Watch
- dashboard `Focus` tetap menaruh `FINAL` dan `WHY` di atas, lalu menambah `Performance`, `Signal Grade`, dan `Volatility`
- dashboard bisa dipindah lewat `Dashboard position`
- `Signal Grade` dan `Volatility` hanya display-only; tidak mengubah confirmed signal, Spec Watch, alert, cooldown, risk, target, atau no-trade logic

Pilih v7.3 kalau kamu ingin alur clean/spec-watch v7.2, tapi tidak mau ingat manual set `Performance mode` ke `Fast` saat pindah ke 1H atau timeframe lebih besar.

### `crypto_amt_toolkit_v5_signal_engine.pine`

Versi kandidat eksperimen. Fokusnya bukan nambah indikator baru, tapi bikin decision layer lebih jelas. File ini tetap dipertahankan apa adanya sebagai pembanding untuk v6 stable-alert.

- ada alur Setup -> Trigger -> Confirmed -> Invalidation
- ada trade mode `Scalp`, `Intraday`, dan `Swing`
- alert dipisah jadi Early Warning, Confirmed Long, Confirmed Short, dan Invalidated
- dashboard menampilkan score parts, active side, block reason, invalidation/target sesuai arah aktif, LTF status, dan flow
- ada risk helper visual untuk invalidation dan target projection
- ada Clean mode untuk mengurangi label historis, rejection mark, dan flow dots saat chart terlalu ramai

Pilih v5 kalau kamu mau test engine sinyal yang lebih rapi dan lebih enak diaudit tanpa guard stable-alert v6. Statusnya masih kandidat eksperimen, jadi bandingkan dulu lawan v3/v4/v6 sebelum dipakai serius. Panduan lengkap cara bacanya ada di [Panduan Pemula Crypto AMT Toolkit v5](docs/crypto-amt-toolkit-v5-panduan-pemula.md).

### `crypto_amt_toolkit_v4_safe_ltf.pine`

Versi kandidat yang lebih baru, arahnya peningkatan dari v3 tapi statusnya masih improved or experimental candidate.

- fokus ke handling lower timeframe yang lebih aman
- ada status LTF dan fallback saat data intrabar tidak ideal
- ada performance mode untuk jaga beban chart
- cocok buat testing improvement tanpa ganggu baseline stabil

Pilih v4 kalau kamu mau coba behavior LTF yang lebih defensif, butuh fallback yang lebih jelas, atau lagi bandingin hasil lawan v3. Kalau ragu, balik ke v3.


### `crypto_amt_toolkit_v2_beginner_colors.pine`

Versi legacy yang lebih visual dan lebih ramah pemula.

- warna level lebih jelas
- label kanan lebih gampang dibaca
- cocok buat belajar mapping VAH, VAL, POC, dan VWAP di chart

Kalau target utama kamu readability visual, v2 masih berguna. Kalau targetnya presisi sinyal, v3 tetap lebih masuk akal.

### `crypto_amt_vwap_vp_orderflow_pa.pine`

Versi baseline atau reference.

- jadi titik awal struktur fitur utama
- berguna buat banding logika dasar sebelum masuk versi toolkit lain
- enak dipakai sebagai acuan kalau mau audit perubahan antar versi

## Batasan Penting

### Orderflow di Pine cuma proxy

Orderflow di script ini bukan real bid or ask DOM. Yang dipakai adalah pendekatan proxy dari data lower timeframe, misalnya delta atau CVD intrabar. Jadi hasilnya bantu baca tekanan beli jual, tapi jangan dianggap data tape asli.

### Custom Volume Profile dan VWAP bands itu pendekatan

Custom VP, POC, VAH, VAL, dan VWAP bands di repo ini adalah aproksimasi berbasis logic Pine. Hasilnya tidak dijamin identik 1 banding 1 dengan built in TradingView.

Alasannya sederhana:

- built in TradingView punya engine internal sendiri
- rendering dan agregasi volume bisa beda
- script Pine punya batas resource, array, dan cara gambar sendiri

Jadi pakai script ini buat toolkit analisis, bukan buat ngejar kecocokan piksel penuh dengan built in.

## Pakai Versi Yang Mana?

- pakai `crypto_amt_toolkit_RECOMMENDED.pine` kalau kamu pemula dan ingin mulai dari pilihan stabil tanpa mikir versi
- pakai `crypto_amt_session_bias_v1.pine` kalau kamu ingin indikator khusus session sweep, reclaim, dan bias Asia/London/New York yang ringan
- pakai `crypto_amt_session_bias_v2_companion.pine` kalau kamu ingin session bias standalone yang lebih pas dipasang di samping v6.1, v6.2, v7.2, atau v7.3, dengan dashboard default `Bottom Right`, detail `Compact`, dan pembaruan previous completed level yang hanya terjadi saat raw session end
- pakai `crypto_amt_ict_smc_companion_v1.pine` kalau kamu ingin companion ICT/SMC compact untuk premium/discount, FVG + CE, IFVG, scored OB, dan CISD display-only tanpa mengganti logic toolkit utama
- pakai `crypto_amt_toolkit_v7_3_auto_performance.pine` kalau kamu mau v7.2 clean/spec-watch dengan default auto-performance, dashboard position, Signal Grade, dan Volatility display-only
- pakai `crypto_amt_toolkit_v7_2_spec_watch.pine` kalau kamu mau memantau kandidat realtime default-off sebelum candle close, tanpa menambah alert Spec Watch
- pakai `crypto_amt_toolkit_v7_1_conservative_clean.pine` kalau kamu mau eksperimen conservative-risk dengan tampilan clean dan dashboard Focus/Full ala v7
- pakai `crypto_amt_toolkit_v7_clean_execution.pine` kalau kamu mau behavior close-confirm ala v6, tapi dengan default chart lebih bersih dan dashboard `Focus` yang menaruh `FINAL` dan `WHY` paling atas
- pakai `crypto_amt_toolkit_v6_2_info_auto_perf.pine` kalau kamu mau v6.1 yang tetap ramai informasi tapi lebih aman di 1H karena default auto-performance
- pakai `crypto_amt_toolkit_v6_1_conservative_risk.pine` kalau kamu mau eksperimen filter volume/risk yang lebih konservatif tanpa mengubah v6 lama
- pakai `crypto_amt_toolkit_v6_stable_alert.pine` kalau mau decision layer v5 dengan alert close-confirm yang lebih aman dan visual default lebih ringan
- pakai `crypto_amt_toolkit_v3_precision.pine` kalau mau versi utama yang stabil dan direkomendasikan
- pakai `crypto_amt_toolkit_v5_signal_engine.pine` kalau mau test decision layer Setup, Trigger, Confirmed, Invalidation, plus risk helper dalam bentuk eksperimen lama yang tidak diubah
- pakai `crypto_amt_toolkit_v4_safe_ltf.pine` kalau mau coba improvement LTF, fallback, dan mode performa, tapi siap terima status kandidat eksperimen
- pakai `crypto_amt_toolkit_v2_beginner_colors.pine` kalau fokus ke visual belajar dan baca level
- pakai `crypto_amt_vwap_vp_orderflow_pa.pine` kalau butuh baseline reference

## Validasi Manual Yang Disarankan

Sebelum dipakai serius, cek manual langsung di TradingView Pine Editor:

1. buka file yang mau dites di Pine Editor
2. compile lalu pastikan script masuk sebagai indikator, bukan strategy
3. pasang ke chart beberapa timeframe
4. cek apakah level VP, VWAP, signal, dan dashboard muncul sesuai ekspektasi
5. khusus Session Bias v1, cek timezone dan jam session, pastikan overlap mengikuti prioritas New York > London > Asia
6. khusus Session Bias v1, cek current high/low/mid/open, opening range, previous high/low, marker Sweep High/Sweep Low, reclaim, bias, dashboard, dan alertcondition muncul sesuai ekspektasi
7. khusus Session Bias v2 Companion, cek bahwa script tetap masuk sebagai `indicator(...)`, bukan `strategy(...)`, lalu pastikan dashboard default muncul di `Bottom Right` dengan detail `Compact`
8. khusus Session Bias v2 Companion, cek bahwa previous completed high/low tidak berubah saat sesi overlap hanya karena prioritas aktif pindah, lalu pastikan level baru hanya update setelah raw session end Asia, London, atau New York benar-benar selesai
9. khusus Session Bias v2 Companion, cek empat alert saja yang tersedia, `Sweep High`, `Sweep Low`, `Reclaim After Sweep`, dan `Bias Flip`, lalu pastikan behavior close-confirm tetap menunggu candle close saat opsi confirm aktif
10. khusus ICT SMC Companion v1, cek bahwa dashboard default muncul di `Bottom Left`, FVG/CE dan IFVG tampil sesuai toggle, scored OB tidak membuat order/backtest, CISD hanya display-only, dan alert TradingView cuma menyediakan dua setup close-confirm
11. khusus v4, cek juga status LTF, fallback, dan mode performanya
12. khusus v5, cek apakah Early Warning, Confirmed, Invalidated, active side, block reason, dan risk helper muncul masuk akal
13. khusus v6, cek `PENDING LONG` / `PENDING SHORT` saat candle realtime belum close, lalu pastikan Confirmed dan Invalidated baru aktif setelah close saat close-confirm menyala
14. khusus v7, cek `Dashboard detail` mode `Focus` dan `Full`, pastikan `FINAL` dan `WHY` muncul di atas detail lain pada mode `Focus`, lalu pastikan behavior `PENDING LONG` / `PENDING SHORT`, Confirmed, dan Invalidated tetap mengikuti close-confirm seperti v6
15. khusus v6.1, cek `Risk mode` default `Structure`, lalu bandingkan `Hybrid` dan `Tight`; pastikan Scalp lebih ketat terhadap relative volume, low-relvol tetap tidak memblok dekat key level, dan `AWAY FROM LEVEL` hanya lolos saat impulse exception yang close-confirmed
16. khusus v6.2, cek `Performance mode = Auto` di 1H dan timeframe kecil; pastikan dashboard menampilkan resolved mode, `Signal Grade`, `Volatility`, dan posisi dashboard sesuai input
17. khusus v7.1, cek semua poin v6.1 plus `Dashboard detail` mode `Focus` / `Full`; pastikan tampilan default tetap bersih seperti v7
18. khusus v7.2, nyalakan `Enable Spec Watch (realtime only)` di market realtime, pastikan status `SPEC WATCH LONG` / `SPEC WATCH SHORT` hanya muncul sebelum candle close, lalu coba `Tint Spec Watch bars amber` tanpa mengganggu bar color normal saat tint off
19. khusus v7.3, cek semua poin v7.2 plus `Performance mode = Auto`, dashboard position, `Signal Grade`, dan `Volatility`; pastikan info baru tidak membuat alert tambahan
20. aktifkan atau matikan Clean mode atau toggle visual lain kalau chart terlalu ramai, lalu pastikan dashboard dan level tag utama tetap terbaca

Intinya, repo ini sekarang diposisikan sebagai toolkit indikator Pine untuk baca chart crypto, bukan mesin backtest otomatis.
