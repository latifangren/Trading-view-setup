# TradingView Pine Indicator Toolkit

Repo ini berisi kumpulan indikator TradingView Pine Script untuk bantu baca chart crypto dengan kombinasi VWAP, Volume Profile, orderflow proxy, price action, dan dashboard confluence.

Semua script di repo ini adalah `indicator(...)`, bukan `strategy(...)`. Artinya script tidak membuat order fill, tidak menjalankan backtest otomatis, dan tidak menghasilkan laporan Strategy Tester. Signal di sini dipakai sebagai alat bantu analisis chart.

## Struktur Repo

```text
indicator/
  README.md
  crypto_amt_session_bias_v1.pine
  crypto_amt_session_bias_v2_companion.pine
  crypto_amt_ict_smc_companion_v1.pine
  crypto_amt_toolkit_RECOMMENDED.pine
  crypto_amt_toolkit_v7_3_auto_performance.pine
  crypto_amt_toolkit_v7_2_spec_watch.pine
  crypto_amt_toolkit_v7_1_conservative_clean.pine
  crypto_amt_toolkit_v7_clean_execution.pine
  crypto_amt_toolkit_v6_2_info_auto_perf.pine
  crypto_amt_toolkit_v6_1_conservative_risk.pine
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
- `indicator/crypto_amt_session_bias_v2_companion.pine` adalah companion session bias generasi baru yang tetap standalone sebagai `indicator(...)`, dibuat untuk dipasang berdampingan dengan toolkit modern v6.1, v6.2, v7.2, atau v7.3. Default dashboard-nya `Bottom Right`, detail default `Compact`, alert tetap close-confirm by default, dan perilaku overlap diperbaiki supaya level sesi selesai sebelumnya hanya update saat sesi mentah benar-benar berakhir.
- `indicator/crypto_amt_ict_smc_companion_v1.pine` adalah companion ICT/SMC standalone yang compact untuk baca premium/discount, FVG + CE, IFVG, scored order block, CISD display-only, dashboard kecil, dan dua alert setup close-confirm tanpa mengubah toolkit utama.
- `indicator/crypto_amt_toolkit_v7_3_auto_performance.pine` adalah v7.3 auto-performance: turunan v7.2 dengan `Performance mode` default `Auto`, posisi dashboard pilihan, serta info display-only `Signal Grade` dan `Volatility`.
- `indicator/crypto_amt_toolkit_v7_2_spec_watch.pine` adalah v7.2 spec-watch: turunan v7.1 dengan opsi default-off `Spec Watch` realtime-only dan tint amber opsional untuk kandidat yang masih menunggu candle close.
- `indicator/crypto_amt_toolkit_v7_1_conservative_clean.pine` adalah v7.1 conservative-clean: logic eksperimen v6.1 dengan default visual dan dashboard Focus/Full ala v7 untuk dicoba tanpa mengubah v7 lama.
- `indicator/crypto_amt_toolkit_v7_clean_execution.pine` adalah v7 clean-execution berbasis v6 stable-alert. Alert close-confirm tetap dipertahankan, default visual dibuat lebih bersih, dan dashboard Focus menaruh `FINAL` serta `WHY` di atas detail raw trigger dan debug.
- `indicator/crypto_amt_toolkit_v6_2_info_auto_perf.pine` adalah v6.2 info auto-performance: turunan v6.1 dengan tampilan informasi tetap kaya, `Performance mode` default `Auto`, posisi dashboard pilihan, serta `Signal Grade` dan `Volatility` display-only.
- `indicator/crypto_amt_toolkit_v6_1_conservative_risk.pine` adalah v6.1 conservative-risk berbasis v6 stable-alert. Ini file eksperimen untuk scalp relative volume lebih ketat, no-trade low-volume yang tetap menghormati key level, dan `Risk mode` Structure/Hybrid/Tight.
- `indicator/crypto_amt_toolkit_v6_stable_alert.pine` adalah v6 stable-alert berbasis v5 dengan alert Confirmed/Invalidated yang default-nya menunggu candle close, status dashboard `PENDING LONG` / `PENDING SHORT`, dan default visual yang lebih ringan.
- `indicator/crypto_amt_toolkit_v3_precision.pine` adalah versi stabil yang paling direkomendasikan untuk pemakaian harian.
- `indicator/crypto_amt_toolkit_v5_signal_engine.pine` adalah kandidat eksperimen yang tetap tidak diubah, dengan decision layer Setup, Trigger, Confirmed, Invalidation, alert terpisah, dan risk helper visual.
- `indicator/crypto_amt_toolkit_v4_safe_ltf.pine` adalah kandidat improvement dengan status LTF/fallback dan performance mode. Pakai ini kalau mau eksperimen fitur terbaru.
- `indicator/crypto_amt_toolkit_v2_beginner_colors.pine` adalah versi legacy yang lebih visual dan ramah pemula.
- `indicator/crypto_amt_vwap_vp_orderflow_pa.pine` adalah baseline/reference untuk audit logika dasar.

Detail tiap file ada di [indicator/README.md](indicator/README.md). Untuk belajar membaca decision layer dari nol, panduan v5 masih relevan sebagai dasar di [Panduan Pemula Crypto AMT Toolkit v5](indicator/docs/crypto-amt-toolkit-v5-panduan-pemula.md). Kalau fokusmu khusus ke companion session bias baru, lihat juga [Panduan Pemula Crypto AMT Session Bias v2 Companion](indicator/docs/crypto-amt-session-bias-v2-companion-panduan-pemula.md). Kalau ingin companion ICT/SMC baru, mulai dari [Panduan Pemula Crypto AMT ICT SMC Companion v1](indicator/docs/crypto-amt-ict-smc-companion-v1-panduan-pemula.md).

## Cara Pakai

1. Untuk mulai paling aman, buka `indicator/crypto_amt_toolkit_RECOMMENDED.pine`.
2. Kalau fokusnya session bias ICT/AMT yang ringan tanpa Volume Profile, buka `indicator/crypto_amt_session_bias_v1.pine`.
3. Kalau kamu sudah pakai toolkit modern v6.1, v6.2, v7.2, atau v7.3 dan ingin pembacaan session bias yang berdampingan tanpa ikut logic toolkit, buka `indicator/crypto_amt_session_bias_v2_companion.pine`.
4. Kalau ingin companion ICT/SMC yang compact untuk FVG, IFVG, premium/discount, scored OB, dan CISD tanpa mengganti toolkit utama, buka `indicator/crypto_amt_ict_smc_companion_v1.pine`.
5. Kalau ingin memilih versi manual, buka file `.pine` lain yang ingin dipakai.
6. Copy isi file ke TradingView Pine Editor.
7. Compile script di TradingView.
8. Add to chart sebagai indikator.
9. Cek hasilnya di beberapa timeframe sebelum dipakai serius.

Untuk pemakaian normal confluence, mulai dari v3 atau RECOMMENDED. Kalau ingin fokus khusus ke session sweep dan reclaim yang lama dan tetap ringan, pakai Session Bias v1, file ini tetap tidak berubah. Kalau ingin companion session bias yang lebih pas dipasang berdampingan dengan toolkit modern, pakai Session Bias v2 Companion bersama v6.1, v6.2, v7.2, atau v7.3. Session Bias v2 tetap standalone, bukan bagian toolkit, jadi kamu bisa menaruhnya di chart yang sama untuk baca konteks sesi Asia, London, dan New York tanpa mengubah logic confluence utama. Default dashboard v2 ada di `Bottom Right` dengan detail `Compact`, semantics overlap sudah dibetulkan, prioritas display aktif tetap New York > London > Asia, tapi level previous completed session hanya ikut berganti saat raw session end asli terjadi, bukan saat prioritas aktif pindah karena overlap. Alert v2 juga sengaja diringkas jadi empat jenis saja, `Sweep High`, `Sweep Low`, `Reclaim After Sweep`, dan `Bias Flip`, dengan close-confirm default tetap menyala. Kalau ingin tambahan konteks ICT/SMC yang compact, pakai ICT SMC Companion v1 di samping toolkit utama; file ini membaca premium/discount, FVG + CE, IFVG, scored OB, dan CISD tanpa mengubah logic sinyal v6/v7. Kalau ingin coba guard lower timeframe dan mode performa yang lebih aman, bandingkan v3 dengan v4. Kalau ingin decision layer v5 tapi alert lebih aman untuk realtime, pakai v6 stable-alert. Kalau ingin alur eksekusi yang sama tapi tampilan default lebih bersih dan dashboard lebih fokus ke keputusan akhir, pakai v7 clean-execution. Kalau mau mengetes filter volume/risk yang lebih konservatif tanpa mengubah versi lama, pakai v6.1 conservative-risk atau v7.1 conservative-clean. Pakai v6.2 kalau ingin layar tetap kaya informasi seperti v6.1 tapi `Performance mode` otomatis turun ke `Fast` pada 1H ke atas. Pakai v7.3 kalau ingin clean dashboard v7.2 plus auto-performance dan info grade/regime. Pakai v7.2/v7.3 spec-watch hanya kalau ingin memantau kandidat realtime default-off sebelum candle close; fitur ini bisa repaint dan tidak menambah alert TradingView. Di keluarga v6/v6.1/v6.2/v7/v7.1/v7.2/v7.3, sinyal mentah intrabar akan tampil sebagai `PENDING LONG` atau `PENDING SHORT` sampai candle close; alert Confirmed Long, Confirmed Short, dan Invalidated baru aktif setelah close saat `Confirm signals on bar close` menyala. Early Warning tetap setup warning awal, bukan alert close-confirm.

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
- Session Bias v2 Companion: companion standalone untuk dipasang di samping v6.1/v6.2/v7.2/v7.3, default dashboard `Bottom Right`, detail `Compact`, empat alert close-confirm, dan update previous completed level hanya saat raw session end
- ICT SMC Companion v1: companion standalone compact untuk premium/discount, FVG + CE, IFVG, scored OB, CISD display-only, dashboard kecil, dan dua alert setup close-confirm
- v7.3: auto-performance berbasis v7.2, dengan default `Auto -> Fast` di 1H ke atas, dashboard position, Signal Grade, Volatility, dan Spec Watch tetap default-off
- v7.2: spec-watch experiment berbasis v7.1, dengan kandidat realtime-only default-off dan tint amber opsional, tanpa alert Spec Watch
- v7.1: conservative-clean experiment, logic v6.1 dengan default visual dan dashboard Focus/Full ala v7
- v7: clean-execution berbasis v6 stable-alert, close-confirm tetap on, visual default lebih bersih, dashboard Focus menaruh `FINAL` dan `WHY` paling atas
- v6.2: info auto-performance berbasis v6.1, dengan tampilan kaya informasi, dashboard position, Signal Grade, Volatility, dan default `Auto -> Fast` di 1H ke atas
- v6.1: conservative-risk experiment berbasis v6, dengan scalp relative volume lebih ketat dan Risk mode Structure/Hybrid/Tight
- v6: stable-alert berbasis v5, dengan close-confirm alert default dan tampilan lebih ringan
- v3: stable/recommended
- v5: signal-engine candidate/experimental, tetap ada dan tidak diubah
- v4: improved candidate/experimental
- v2: legacy visual
- baseline: reference

Kalau nanti ada improvement baru, buat file versi baru daripada mengubah versi lama, supaya perilaku tiap versi tetap bisa dibandingkan.

Riwayat perubahan utama dicatat di [CHANGELOG.md](CHANGELOG.md). Lisensi repo ada di [LICENSE](LICENSE).
