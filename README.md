# TradingView Pine Indicator Toolkit

Repo ini berisi kumpulan indikator TradingView Pine Script untuk bantu baca chart crypto dengan kombinasi VWAP, Volume Profile, orderflow proxy, price action, dan dashboard confluence.

Semua script di repo ini adalah `indicator(...)`, bukan `strategy(...)`. Artinya script tidak membuat order fill, tidak menjalankan backtest otomatis, dan tidak menghasilkan laporan Strategy Tester. Signal di sini dipakai sebagai alat bantu analisis chart.

## Struktur Repo

```text
indicator/
  README.md
  crypto_amt_toolkit_v5_signal_engine.pine
  crypto_amt_toolkit_v4_safe_ltf.pine
  crypto_amt_toolkit_v3_precision.pine
  crypto_amt_toolkit_v2_beginner_colors.pine
  crypto_amt_vwap_vp_orderflow_pa.pine
```

Folder `indicator/` adalah lokasi utama script Pine. Folder `strategy/` sudah tidak dipakai untuk flow utama karena nama itu bisa menyesatkan: file-file ini bukan strategy backtest.

## Pilih File Yang Mana?

- `indicator/crypto_amt_toolkit_v3_precision.pine` adalah versi stabil yang paling direkomendasikan untuk pemakaian harian.
- `indicator/crypto_amt_toolkit_v5_signal_engine.pine` adalah kandidat eksperimen terbaru dengan decision layer Setup, Trigger, Confirmed, Invalidation, alert terpisah, dan risk helper visual.
- `indicator/crypto_amt_toolkit_v4_safe_ltf.pine` adalah kandidat improvement dengan status LTF/fallback dan performance mode. Pakai ini kalau mau eksperimen fitur terbaru.
- `indicator/crypto_amt_toolkit_v2_beginner_colors.pine` adalah versi legacy yang lebih visual dan ramah pemula.
- `indicator/crypto_amt_vwap_vp_orderflow_pa.pine` adalah baseline/reference untuk audit logika dasar.

Detail tiap file ada di [indicator/README.md](indicator/README.md). Untuk belajar membaca v5 dari nol, buka [Panduan Pemula Crypto AMT Toolkit v5](indicator/docs/crypto-amt-toolkit-v5-panduan-pemula.md).

## Cara Pakai

1. Buka file `.pine` yang ingin dipakai.
2. Copy isi file ke TradingView Pine Editor.
3. Compile script di TradingView.
4. Add to chart sebagai indikator.
5. Cek hasilnya di beberapa timeframe sebelum dipakai serius.

Untuk pemakaian normal, mulai dari v3. Kalau ingin coba guard lower timeframe dan mode performa yang lebih aman, bandingkan v3 dengan v4. Kalau ingin eksperimen decision layer yang lebih jelas, bandingkan v3/v4 dengan v5 di chart yang sama.

## Validasi Lokal

Repo ini sudah dicoba dengan validator lokal Pine Script v6:

```powershell
pine-validator indicator --agent-json --no-hints --no-information
```

Hasil terakhir: semua file `indicator/*.pine` lolos dengan `0 error` dan `0 warning`.

Catatan: validator lokal membantu preflight, tapi bukan compiler resmi TradingView. Compile final tetap harus dicek di TradingView Pine Editor.

## Batasan Penting

Orderflow di script ini adalah proxy, bukan data bid/ask DOM asli. Lower timeframe delta dan CVD membantu membaca tekanan beli/jual, tapi tidak menggantikan tape atau order book real.

Custom Volume Profile, POC, VAH, VAL, dan VWAP bands juga merupakan pendekatan berbasis Pine. Hasilnya tidak dijamin identik 1:1 dengan built-in TradingView karena engine internal, agregasi volume, dan batas resource Pine bisa berbeda.

## Status Versi

- v3: stable/recommended
- v5: signal-engine candidate/experimental
- v4: improved candidate/experimental
- v2: legacy visual
- baseline: reference

Kalau nanti ada improvement baru, buat file versi baru daripada mengubah versi lama, supaya perilaku tiap versi tetap bisa dibandingkan.