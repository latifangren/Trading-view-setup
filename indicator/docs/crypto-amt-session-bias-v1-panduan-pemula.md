# Panduan Pemula Crypto AMT Session Bias v1

Indikator ini dipakai untuk membaca konteks sesi Asia, London, dan New York. Fokusnya bukan memberi entry otomatis, tapi membantu kamu tahu apakah harga sedang membangun range, menyapu level sesi sebelumnya, atau sudah reclaim dan mulai memberi bias arah.

Script ini adalah `indicator(...)`, bukan `strategy(...)`. Artinya tidak ada backtest otomatis, tidak ada order fill, dan tidak ada jaminan profit.

## Yang Wajib Dipahami

### Active Session

Dashboard menampilkan sesi yang sedang aktif: Asia, London, New York, atau None. Default timezone adalah `Etc/UTC`. Kalau chart kamu terasa tidak cocok dengan jam market yang biasa kamu pakai, cek input `Session timezone`.

Kalau session input saling overlap, script memilih prioritas New York, lalu London, lalu Asia.

### Current Session High / Low

Ini adalah titik tertinggi dan terendah dari sesi yang sedang berjalan. Level ini membantu melihat range hari itu sedang melebar ke atas atau ke bawah.

### Current Mid / Open

Mid adalah tengah range sesi berjalan. Open adalah harga pembukaan sesi. Harga di atas mid/open biasanya lebih kuat secara intraday, sedangkan harga di bawahnya lebih lemah. Jangan pakai ini sendirian untuk entry.

### Opening Range

Opening range adalah range awal sesi sesuai input `Opening range minutes`. Default 60 menit. High dan low opening range sering jadi area breakout, fakeout, atau retest.

### Previous Session High / Low

Ini adalah high dan low dari sesi aktif sebelumnya yang sudah selesai. Level ini sering jadi target liquidity. Misalnya London bisa menyapu Asia high atau Asia low sebelum menentukan arah.

## Cara Baca Sweep dan Reclaim

### Sweep High

`Sweep High` muncul saat harga menembus high sesi sebelumnya. Ini bukan otomatis short. Artinya harga mengambil liquidity di atas high lama.

Yang perlu dilihat setelah sweep high:

- apakah harga lanjut acceptance di atas level itu
- atau harga balik turun lagi ke bawah level itu

Kalau harga balik close di bawah previous high setelah sweep high, dashboard bisa berubah ke bearish reclaim.

### Sweep Low

`Sweep Low` muncul saat harga menembus low sesi sebelumnya. Ini bukan otomatis long. Artinya harga mengambil liquidity di bawah low lama.

Yang perlu dilihat setelah sweep low:

- apakah harga lanjut acceptance di bawah level itu
- atau harga balik naik lagi ke atas level itu

Kalau harga balik close di atas previous low setelah sweep low, dashboard bisa berubah ke bullish reclaim.

### Reclaim After Sweep

Reclaim adalah momen harga kembali masuk ke range sesi sebelumnya setelah sweep. Ini sering lebih penting daripada sweep itu sendiri.

- Sweep low lalu reclaim ke atas previous low = bias bullish
- Sweep high lalu reclaim ke bawah previous high = bias bearish

### Bias Flip

`Bias Flip` muncul saat reclaim mengubah bias session menjadi bullish atau bearish. Ini membantu filtering: cari long lebih hati-hati saat bias bearish, dan cari short lebih hati-hati saat bias bullish.

## Alert Yang Tersedia

- `AMT Session Bias v1 Sweep High`
- `AMT Session Bias v1 Sweep Low`
- `AMT Session Bias v1 Reclaim After Sweep`
- `AMT Session Bias v1 Bias Flip`

Input `Confirm alerts on bar close` default aktif. Kalau aktif, alert menunggu candle close supaya tidak terlalu cepat berubah saat candle realtime masih berjalan.

## Cara Pakai Bareng Toolkit Lain

Workflow sederhana:

1. Pakai Session Bias v1 untuk lihat sesi aktif dan sweep/reclaim.
2. Pakai v3 sebagai indikator stabil untuk confluence utama.
3. Pakai v6 kalau ingin decision layer dan alert confirmed yang lebih rapi.
4. Jangan entry hanya karena sweep. Tunggu reclaim, lokasi bagus, dan konfirmasi dari toolkit lain.

## Batasan

- Session default memakai UTC, bukan timezone lokal kamu.
- Opening range lebih akurat di timeframe intraday kecil seperti 5m, 15m, atau 30m.
- Di timeframe besar, satu candle bisa melewati batas session sehingga pembacaan bisa kurang presisi.
- Sweep/reclaim adalah konteks market, bukan sinyal entry otomatis.
