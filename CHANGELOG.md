# Changelog

Catatan perubahan penting untuk TradingView Pine Indicator Toolkit.

## Unreleased

- Added `indicator/crypto_amt_toolkit_v7_2_spec_watch.pine` as a v7.1-based Spec Watch experiment for realtime-only default-off candidates.
- Added optional amber bar tint for v7.2 Spec Watch while keeping existing confirmed alerts and active plan state unchanged.
- Documented v7.2 chooser guidance and manual validation checklist in the root and indicator READMEs.
- Added `indicator/crypto_amt_toolkit_v6_1_conservative_risk.pine` as a conservative-risk experiment based on v6 stable-alert.
- Added `indicator/crypto_amt_toolkit_v7_1_conservative_clean.pine` as a clean-dashboard conservative experiment based on v7 clean-execution.
- Added stricter scalp relative-volume gating, key-level-aware low-relvol blocking, limited close-confirm impulse exception for away-from-level guards, and `Risk mode` Structure/Hybrid/Tight in v6.1/v7.1.
- Kept existing v6 and v7 files unchanged so the stable-alert and clean-execution baselines remain comparable.
- Added `indicator/crypto_amt_session_bias_v1.pine` as a new Pine v6 overlay indicator for Asia/London/New York session bias.
- Added Session Bias v1 sweep, reclaim, bias flip markers, top-right dashboard, and TradingView alerts.
- Documented Session Bias v1 chooser guidance and manual validation checklist in the root and indicator READMEs.
- Added GitHub Actions CI to run Pine validator on `indicator/*.pine` for `push` and `pull_request`.
- Added `indicator/crypto_amt_toolkit_v6_stable_alert.pine` from v5 as a stable-alert variant with `Confirm signals on bar close` enabled by default.
- Added gated v6 Confirmed Long, Confirmed Short, and Invalidated alerts so they wait for candle close unless close-confirm is disabled.
- Added v6 dashboard distinction for realtime raw candidates as `PENDING LONG` / `PENDING SHORT` plus a `Close Confirm` status row.
- Changed v6 defaults to a lighter chart: rejection marks off, flow dots off, and Clean mode on.
- Added `indicator/crypto_amt_toolkit_v7_clean_execution.pine` as a clean-execution variant based on v6 stable-alert.
- Kept v7 close-confirm behavior aligned with v6, including `PENDING LONG` / `PENDING SHORT` before candle close and confirmed or invalidated alerts after close.
- Added v7 dashboard `Focus` / `Full` detail modes, with `FINAL` and `WHY` promoted above raw trigger and debug-heavy rows in `Focus` mode.
- Changed v7 defaults to a cleaner chart by turning VP boxes, VWAP bands, EMAs, labels, risk helper, bar color, level tags, rejection marks, and flow dots off while keeping the dashboard on.
- Kept `indicator/crypto_amt_toolkit_v5_signal_engine.pine` unchanged as the experimental signal-engine reference.
- Added `indicator/crypto_amt_toolkit_RECOMMENDED.pine` as the beginner-friendly stable entrypoint copied from v3 Precision.
- Documented the recommended entrypoint in the root README.
- Added a clearer educational-use disclaimer in the root README.

## v5 - Signal Engine

- Added Setup -> Trigger -> Confirmed -> Invalidated decision flow.
- Added trade modes: Scalp, Intraday, and Swing.
- Added separated alerts for Early Warning, Confirmed Long, Confirmed Short, and Invalidated.
- Added active side, block reason, and risk/target helper visuals.
- Added Clean mode to reduce chart clutter.

## v4 - Safe LTF

- Added lower timeframe status and fallback handling.
- Added performance mode for safer lower-timeframe usage.
- Improved dashboard visibility for LTF delta state.

## v3 - Precision

- Added stricter confluence scoring.
- Added EMA trend filter and HTF bias.
- Added no-trade filtering for middle value area.
- Added clearer dashboard fields for bias, location, trend, flow, and price action.

## v2 - Beginner Colors

- Improved beginner-friendly colors for POC, VAH, VAL, VWAP, and signals.
- Added right-side labels for important levels.
- Added clearer visual markers for reading chart context.

## Baseline

- Initial AMT, VWAP, Volume Profile, orderflow proxy, and price action confluence indicator.
