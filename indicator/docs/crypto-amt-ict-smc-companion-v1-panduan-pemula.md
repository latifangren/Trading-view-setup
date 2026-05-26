# Panduan Pemula Crypto AMT ICT SMC Companion v1

File: `indicator/crypto_amt_ict_smc_companion_v1.pine`

ICT SMC Companion v1 adalah indikator pendamping. Pakai file ini sebagai layer konteks tambahan di samping toolkit utama seperti v6.2 atau v7.3, bukan sebagai pengganti sinyal utama.

Script ini tetap `indicator(...)`, bukan `strategy(...)`. Tidak ada order otomatis, backtest, sizing, leverage, atau tracker TP/SL.

## Yang Dibaca

- `Range Zone`: posisi harga terhadap premium, discount, atau equilibrium.
- `FVG`: jumlah bullish dan bearish fair value gap aktif.
- `IFVG`: FVG yang sudah ditembus close-confirm lalu berubah peran menjadi inversion FVG.
- `OB Score`: score order block aktif terbaik untuk sisi bullish dan bearish.
- `CISD`: change in state of delivery sebagai info visual saja.
- `Setup Bias`: ringkasan bias companion berdasarkan premium/discount, FVG, IFVG, dan scored OB.

## Cara Pakai Cepat

1. Buka `indicator/crypto_amt_ict_smc_companion_v1.pine`.
2. Copy isi file ke TradingView Pine Editor.
3. Compile dan add to chart sebagai indikator.
4. Pasang berdampingan dengan toolkit utama bila perlu.
5. Cek dashboard default di `Bottom Left`.
6. Pastikan hanya dua alert tersedia: bullish setup dan bearish setup.

## Premium dan Discount

Premium berarti harga berada di atas equilibrium range. Discount berarti harga berada di bawah equilibrium range.

Companion memakai swing terbaru bila sudah tersedia. Kalau swing belum terbentuk, script memakai fallback range dari jumlah bar terakhir.

Gunakan premium/discount sebagai konteks, bukan tombol entry. Misalnya setup long lebih sehat saat ada konteks discount, sedangkan setup short lebih sehat saat ada konteks premium.

## FVG dan CE

FVG dibuat dari gap tiga candle dengan filter displacement ATR. Artinya gap kecil tanpa dorongan candle yang cukup bisa diabaikan.

CE adalah midpoint 50% dari FVG. Garis ini membantu melihat apakah harga sudah masuk terlalu dalam ke gap.

## IFVG

IFVG muncul saat FVG aktif ditembus dengan candle close ke sisi sebaliknya.

- Bullish IFVG berarti bearish FVG lama flip menjadi support context.
- Bearish IFVG berarti bullish FVG lama flip menjadi resistance context.

IFVG dibuat close-confirm supaya tidak terlalu mudah berubah saat candle masih berjalan.

## Scored OB

Order block di companion ini sengaja dibuat ringan. Score dibangun dari opposite candle terbaru setelah displacement FVG, premium/discount alignment, dan freshness.

Score ini hanya konteks kualitas zona. Tidak ada order otomatis, tidak ada saran sizing, dan tidak ada perhitungan profit.

## CISD

CISD hanya display-only. Marker dan dashboard memberi tahu apakah ada shift delivery bullish atau bearish, tapi CISD tidak ikut mengubah score setup atau alert companion.

## Alert

Alert sengaja dibatasi menjadi dua:

- `AMT ICT SMC v1 Bullish Setup`
- `AMT ICT SMC v1 Bearish Setup`

Default `Confirm alerts on bar close` menyala. Dengan default ini, alert menunggu candle close.

## Checklist Manual

Sebelum dipakai serius:

1. Compile di TradingView Pine Editor.
2. Pastikan script masuk sebagai indikator, bukan strategy.
3. Cek dashboard default `Bottom Left`.
4. Cek FVG, CE, IFVG, dan OB muncul sesuai toggle.
5. Pastikan alert hanya dua jenis setup.
6. Bandingkan dengan toolkit utama di beberapa timeframe.
7. Kalau chart terlalu ramai, turunkan `Max active FVGs`, `Max active IFVGs`, `Max active OBs`, atau matikan visual yang tidak diperlukan.

## Batasan

Companion ini bukan mesin eksekusi. Semua pembacaan tetap perlu dikonfirmasi manual dengan struktur chart, session context, volume, dan risk plan masing-masing.
