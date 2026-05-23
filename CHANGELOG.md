# Changelog

Catatan perubahan penting untuk TradingView Pine Indicator Toolkit.

## Unreleased

- Added GitHub Actions CI to run Pine validator on `indicator/*.pine` for `push` and `pull_request`.
- Added `indicator/crypto_amt_toolkit_v6_stable_alert.pine` from v5 as a stable-alert variant with `Confirm signals on bar close` enabled by default.
- Added gated v6 Confirmed Long, Confirmed Short, and Invalidated alerts so they wait for candle close unless close-confirm is disabled.
- Added v6 dashboard distinction for realtime raw candidates as `PENDING LONG` / `PENDING SHORT` plus a `Close Confirm` status row.
- Changed v6 defaults to a lighter chart: rejection marks off, flow dots off, and Clean mode on.
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
