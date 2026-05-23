# TradingView Pine Indicator Toolkit

Repo ini berisi kumpulan indikator TradingView Pine Script untuk bantu baca chart crypto dengan kombinasi VWAP, Volume Profile, orderflow proxy, price action, dan dashboard confluence.

Semua script di repo ini adalah `indicator(...)`, bukan `strategy(...)`. Artinya script tidak membuat order fill, tidak menjalankan backtest otomatis, dan tidak menghasilkan laporan Strategy Tester. Signal di sini dipakai sebagai alat bantu analisis chart.

## Struktur Repo

```text
indicator/
  README.md
  crypto_amt_session_bias_v1.pine
  crypto_amt_toolkit_RECOMMENDED.pine
  crypto_amt_toolkit_v6_stable_alert.pine
  crypto_amt_toolkit_v5_signal_engine.pine
  crypto_amt_toolkit_v4_safe_ltf.pine
  crypto_amt_toolkit_v3_precision.pine
  crypto_amt_toolkit_v2_beginner_colors.pine
  crypto_amt_vwap_vp_orderflow_pa.pine
```

Folder `indicator/` adalah lokasi utama script Pine. Folder `strategy/` sudah tidak dipakai untuk flow utama karena nama itu bisa menyesatkan: file-file ini bukan strategy backtest.

## Pilih File Yang Mana?

- `indicator/crypto_amt_toolkit_RECOMMENDED.pine` adalah file paling aman untuk pemula. Saat ini isinya sama dengan v3 Precision yang stabil/recommended.
- `indicator/crypto_amt_session_bias_v1.pine` adalah indikator session bias yang lebih ringan untuk baca Asia, London, dan New York: sweep high/low session sebelumnya, reclaim, bias bullish/bearish, dashboard, dan alert.
- `indicator/crypto_amt_toolkit_v6_stable_alert.pine` adalah v6 stable-alert berbasis v5 dengan alert Confirmed/Invalidated yang default-nya menunggu candle close, status dashboard `PENDING LONG` / `PENDING SHORT`, dan default visual yang lebih ringan.
- `indicator/crypto_amt_toolkit_v3_precision.pine` adalah versi stabil yang paling direkomendasikan untuk pemakaian harian.
- `indicator/crypto_amt_toolkit_v5_signal_engine.pine` adalah kandidat eksperimen yang tetap tidak diubah, dengan decision layer Setup, Trigger, Confirmed, Invalidation, alert terpisah, dan risk helper visual.
- `indicator/crypto_amt_toolkit_v4_safe_ltf.pine` adalah kandidat improvement dengan status LTF/fallback dan performance mode. Pakai ini kalau mau eksperimen fitur terbaru.
- `indicator/crypto_amt_toolkit_v2_beginner_colors.pine` adalah versi legacy yang lebih visual dan ramah pemula.
- `indicator/crypto_amt_vwap_vp_orderflow_pa.pine` adalah baseline/reference untuk audit logika dasar.

Detail tiap file ada di [indicator/README.md](indicator/README.md). Untuk belajar membaca decision layer dari nol, panduan v5 masih relevan sebagai dasar di [Panduan Pemula Crypto AMT Toolkit v5](indicator/docs/crypto-amt-toolkit-v5-panduan-pemula.md).

## Cara Pakai

1. Untuk mulai paling aman, buka `indicator/crypto_amt_toolkit_RECOMMENDED.pine`.
2. Kalau fokusnya session bias ICT/AMT yang ringan tanpa Volume Profile, buka `indicator/crypto_amt_session_bias_v1.pine`.
3. Kalau ingin memilih versi manual, buka file `.pine` lain yang ingin dipakai.
4. Copy isi file ke TradingView Pine Editor.
5. Compile script di TradingView.
6. Add to chart sebagai indikator.
7. Cek hasilnya di beberapa timeframe sebelum dipakai serius.

Untuk pemakaian normal confluence, mulai dari v3 atau RECOMMENDED. Kalau ingin fokus khusus ke session sweep dan reclaim, pakai Session Bias v1. Kalau ingin coba guard lower timeframe dan mode performa yang lebih aman, bandingkan v3 dengan v4. Kalau ingin decision layer v5 tapi alert lebih aman untuk realtime, pakai v6 stable-alert. Di v6, sinyal mentah intrabar akan tampil sebagai `PENDING LONG` atau `PENDING SHORT` sampai candle close; alert Confirmed Long, Confirmed Short, dan Invalidated baru aktif setelah close saat `Confirm signals on bar close` menyala. Early Warning tetap setup warning awal, bukan alert close-confirm.

## Validasi Lokal

Repo ini sudah dicoba dengan validator lokal Pine Script v6:

```powershell
pine-validator indicator --agent-json --no-hints --no-information
```

Hasil terakhir: semua file `indicator/*.pine` lolos dengan `0 error` dan `0 warning`.

GitHub Actions juga menjalankan validasi yang sama pada `push` dan `pull_request` lewat `.github/workflows/pine-validator.yml`.

Catatan: validator lokal membantu preflight, tapi bukan compiler resmi TradingView. Compile final tetap harus dicek di TradingView Pine Editor.

## Batasan Penting

Repo ini hanya untuk edukasi dan analisis chart. Script di sini bukan nasihat finansial, tidak menjamin profit, dan tidak boleh dianggap sebagai sistem trading mandiri. Keputusan entry, exit, sizing, dan risiko tetap tanggung jawab masing-masing user.

Orderflow di script ini adalah proxy, bukan data bid/ask DOM asli. Lower timeframe delta dan CVD membantu membaca tekanan beli/jual, tapi tidak menggantikan tape atau order book real.

Custom Volume Profile, POC, VAH, VAL, dan VWAP bands juga merupakan pendekatan berbasis Pine. Hasilnya tidak dijamin identik 1:1 dengan built-in TradingView karena engine internal, agregasi volume, dan batas resource Pine bisa berbeda.

## Status Versi

- RECOMMENDED: alias stabil untuk pemula, saat ini copy dari v3
- Session Bias v1: indikator ringan untuk sesi Asia/London/New York, sweep, reclaim, dan bias intraday
- v6: stable-alert berbasis v5, dengan close-confirm alert default dan tampilan lebih ringan
- v3: stable/recommended
- v5: signal-engine candidate/experimental, tetap ada dan tidak diubah
- v4: improved candidate/experimental
- v2: legacy visual
- baseline: reference

Kalau nanti ada improvement baru, buat file versi baru daripada mengubah versi lama, supaya perilaku tiap versi tetap bisa dibandingkan.

Riwayat perubahan utama dicatat di [CHANGELOG.md](CHANGELOG.md). Lisensi repo ada di [LICENSE](LICENSE).
