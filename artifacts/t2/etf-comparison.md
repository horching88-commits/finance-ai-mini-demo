# T2 ETF Comparison: SPY, TLT, GLD — Fees and Historical Drawdown

This task upgrades the T1 illustrative snapshot plan (synthetic `data/etf_snapshot.csv` assumptions) to a real-data comparison, using the T2 ETF Data Pack with daily prices from 2016-09-01 through 2026-08-31.

## Source and Method

- **Input paths:** `artifacts/t1/project-plan.md` (context only), `data/t2/data_dictionary.md`, `data/t2/fund_info.csv`, `data/t2/daily_prices.csv` (7,536 data rows).
- **Preparation date:** T2 ETF Data Pack prepared 2026-09-25; retained Yahoo Finance chart responses recorded as downloaded 2026-09-20 (source provenance per the data dictionary).
- **Common drawdown period:** 2016-09-01 through 2026-08-31, inclusive; 2,512 observations per ETF; daily frequency (US market session dates, interpreted in America/New_York); adjustment basis: `adjusted_close` (provider split and dividend-distribution adjustments).
- **Fee disclosure dates (per issuer pages, all accessed 2026-09-25):**
  - SPY: Fund-information as-of date 2026-09-10; fee-specific effective date not stated (gross of waivers/reimbursements).
  - TLT: Current prospectus; specific date not stated in the fee panel.
  - GLD: Not stated in the selected field.
- **Script and command:** `artifacts/t2/calculate_drawdown.py`, executed as `python3 artifacts/t2/calculate_drawdown.py` from the repository root (Python 3.9.6, standard library only).
- **Input check result:** PASSED — exactly SPY, TLT, GLD; unique ticker/date pairs; dates ascending within each group; positive finite `close` and `adjusted_close`; 2,512 rows per ETF; common first/last dates 2016-09-01/2026-08-31; identical date sets across tickers.
- **Calculation method:** for each ticker, all `adjusted_close` observations in the window; `drawdown_t = (adjusted_close_t / cumulative_high_t - 1) * 100`, where `cumulative_high_t` is the largest adjusted close from window start through day t; maximum drawdown = minimum drawdown across the window. Peak must be on or before the trough; on ties, the earliest trough and its earliest corresponding peak; if no loss, first date for both and zero drawdown. Full precision kept internally; two-decimal display only.

## 1. Comparison

| Ticker | Annual expense ratio (%) | Maximum drawdown (%) | Peak date | Trough date |
|---|---|---|---|---|
| SPY | 0.0945 | -33.72 | 2020-02-19 | 2020-03-23 |
| TLT | 0.15 | -48.35 | 2020-08-04 | 2023-10-19 |
| GLD | 0.4 | -26.40 | 2026-01-29 | 2026-07-16 |

Fee precision preserved as disclosed (`0.0945%`, `0.15%`, `0.4%`). Drawdowns displayed to two decimals.

## 2. Observation

In this 2016-09-01 to 2026-08-31 window, **GLD** has the smallest drawdown loss (closest to zero) at **-26.40%** among the displayed two-decimal results (SPY -33.72%, TLT -48.35%); there are no ties. These drawdowns were calculated from `adjusted_close`, not read from a precomputed snapshot; they are non-positive, so closer to zero means a smaller loss. Limitation: drawdowns are daily-close measures within this fixed historical window only — they do not capture intraday losses, and the disclosed fees are current disclosure snapshots, not ten-year averages, while these drawdowns are not all-time measures; the inputs are teaching data, not investment advice.

## 3. Agent Check

Self-check performed by the agent on **SPY** (this is the agent's own verification, not an independent external check).

- **Source rows re-read from `data/t2/daily_prices.csv`:**
  - Peak 2020-02-19: `SPY,2020-02-19,close=338.3399963378906,adjusted_close=307.6394958496`
  - Trough 2020-03-23: `SPY,2020-03-23,close=222.9499969482422,adjusted_close=203.9118957520`
- **Check command:** inline Python (standard library) reading `daily_prices.csv`, locating both rows, recomputing `(trough / peak - 1) * 100`, and independently recomputing the full-series cumulative highs.
- **Actual output:** recomputed `(203.9118957520 / 307.6394958496 - 1) * 100 = -33.717257210811%`; full-series recompute gave `best_dd = -33.717257210811%` with peak 2020-02-19 and trough 2020-03-23; peak date ≤ trough date confirmed (2020-02-19 ≤ 2020-03-23); the peak equals the earliest cumulative high on or before the trough (307.6394958496, first achieved 2020-02-19); only one date achieves the most-negative drawdown, so the earliest-trough rule is unaffected.
- **Comparison:** the recomputed value matches the report's -33.72% (and the underlying -33.717257% full precision) exactly.
- **Correction/recheck:** an initial comparison against a rounded six-decimal literal flagged a false mismatch (a display-precision artifact); the recheck against full precision (`.12f`) confirmed exact agreement, so no calculation change was needed. All input checks and all three drawdowns (SPY -33.717257210811%, TLT -48.351126%, GLD -26.404518% full precision) were reviewed against the observation; GLD remains the smallest loss with no ties.
