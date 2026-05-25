# Panduan Pemula Crypto AMT Session Bias v2 Companion

Session Bias v2 Companion dibuat buat kamu yang sudah punya toolkit utama di chart, lalu ingin panel session bias yang berdiri sendiri. Script ini tetap `indicator(...)`, bukan `strategy(...)`, jadi fungsinya bantu baca konteks sesi, bukan backtest otomatis.

## Kapan Pakai v2 Companion

Pakai file ini kalau workflow kamu sudah bertumpu pada salah satu toolkit modern berikut:

1. `crypto_amt_toolkit_v6_1_conservative_risk.pine`
2. `crypto_amt_toolkit_v6_2_info_auto_perf.pine`
3. `crypto_amt_toolkit_v7_2_spec_watch.pine`
4. `crypto_amt_toolkit_v7_3_auto_performance.pine`

V2 Companion cocok dipasang berdampingan dengan file-file itu karena session bias-nya dipisah dari engine confluence utama. Jadi kamu bisa baca sweep, reclaim, dan bias sesi tanpa mengubah perilaku toolkit utama.

Kalau kamu cuma butuh versi ringan lama, pakai `crypto_amt_session_bias_v1.pine`. File v1 tetap ada dan tetap tidak berubah.

## Yang Berubah Dibanding v1

- dashboard default pindah ke `Bottom Right`
- `Dashboard detail` default jadi `Compact`
- overlap semantics dibetulkan
- previous completed session level hanya update saat raw session end asli terjadi
- alert disederhanakan jadi empat jenis inti

Inti perbaikannya ada di overlap. Display aktif tetap ikut prioritas New York, lalu London, lalu Asia. Tapi previous completed high/low tidak ikut lompat hanya karena sesi aktif prioritasnya bergeser saat dua sesi overlap. Level sebelumnya baru diganti saat sesi mentah yang bersangkutan benar-benar selesai.

## Empat Alert Inti

Session Bias v2 Companion cuma menyisakan empat alert ini:

1. `Sweep High`
2. `Sweep Low`
3. `Reclaim After Sweep`
4. `Bias Flip`

Default `Confirm alerts on bar close` tetap aktif. Jadi alert menunggu candle close saat opsi confirm aktif.

## Cara Baca Cepat

### Sweep High

Harga menyapu high sesi selesai sebelumnya. Ini konteks liquidity grab, bukan auto short.

### Sweep Low

Harga menyapu low sesi selesai sebelumnya. Ini konteks liquidity grab, bukan auto long.

### Reclaim After Sweep

Harga masuk lagi ke dalam range setelah sweep. Bagian ini sering lebih penting daripada sweep awalnya.

### Bias Flip

Bias sesi berubah bullish atau bearish setelah reclaim terkonfirmasi.

## Workflow Sederhana

1. Pasang toolkit utama, misalnya v6.2 atau v7.3.
2. Pasang Session Bias v2 Companion di chart yang sama.
3. Biarkan toolkit utama tetap baca confluence, trend, volume, dan eksekusi.
4. Pakai companion untuk lihat apakah sweep dan reclaim sesi mendukung arah setup toolkit.

## Batasan

- script ini tetap indikator, bukan strategy
- tidak ada order fill otomatis
- tidak ada performance report Strategy Tester
- di timeframe besar, satu candle bisa melewati batas session dan membuat pembacaan intraday kurang presisi
- sweep atau bias flip bukan janji entry, tetap butuh konteks market yang layak
