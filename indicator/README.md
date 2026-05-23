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

### `crypto_amt_toolkit_v7_clean_execution.pine`

Versi clean-execution berbasis v6 stable-alert. Fokusnya bukan mengubah decision layer utama, tapi merapikan default chart dan dashboard supaya keputusan akhir lebih cepat terbaca.

- default `Confirm signals on bar close` tetap menyala, jadi behavior close-confirm tetap sama seperti v6
- dashboard tetap on, dan `Dashboard detail` default-nya `Focus`
- mode `Focus` menaruh baris `FINAL` dan `WHY` di bagian paling atas sebelum detail `Close Confirm`, score, context, active, flow, dan raw trigger
- mode `Full` membuka lagi detail debug seperti score parts, setup, block reason, risk, target, LTF delta, delta/relvol, PA, dan mode
- default visual dibuat lebih ringan: VP boxes, VWAP bands, EMA trend filter, setup or confirmed labels, risk helper, bar color, level tags, rejection marks, dan flow dots default off
- Clean mode tetap on supaya chart historis tidak cepat penuh, tapi user masih bisa menyalakan toggle visual satu per satu kalau butuh audit lebih detail

Pilih v7 kalau kamu suka alur close-confirm v6, tapi ingin tampilan yang lebih bersih dan dashboard yang langsung menekankan keputusan akhir serta alasan di baliknya. Ini cocok buat pemakaian harian saat kamu lebih sering butuh ringkasan eksekusi daripada panel debug penuh.

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
- pakai `crypto_amt_toolkit_v7_clean_execution.pine` kalau kamu mau behavior close-confirm ala v6, tapi dengan default chart lebih bersih dan dashboard `Focus` yang menaruh `FINAL` dan `WHY` paling atas
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
7. khusus v4, cek juga status LTF, fallback, dan mode performanya
8. khusus v5, cek apakah Early Warning, Confirmed, Invalidated, active side, block reason, dan risk helper muncul masuk akal
9. khusus v6, cek `PENDING LONG` / `PENDING SHORT` saat candle realtime belum close, lalu pastikan Confirmed dan Invalidated baru aktif setelah close saat close-confirm menyala
10. khusus v7, cek `Dashboard detail` mode `Focus` dan `Full`, pastikan `FINAL` dan `WHY` muncul di atas detail lain pada mode `Focus`, lalu pastikan behavior `PENDING LONG` / `PENDING SHORT`, Confirmed, dan Invalidated tetap mengikuti close-confirm seperti v6
11. aktifkan atau matikan Clean mode atau toggle visual lain kalau chart terlalu ramai, lalu pastikan dashboard dan level tag utama tetap terbaca

Intinya, repo ini sekarang diposisikan sebagai toolkit indikator Pine untuk baca chart crypto, bukan mesin backtest otomatis.
