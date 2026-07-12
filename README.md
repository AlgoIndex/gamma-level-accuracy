# S&P 500 Gamma Level Accuracy Dataset

Forward-tested daily outcomes for S&P 500 dealer-positioning levels: call wall, put wall, gamma flip, expected move band, key gamma strike, and session reference levels (prior-day high/low, value area, volume point of control, overnight range).

Every level is computed from the live option chain or session data BEFORE the trading session, stored, and graded after the close against the completed session's high, low, and close. No level is re-marked after the fact and no grade is ever revised. The record is forward-only: no backtest, no lookahead.

## Live sources

- Live scoreboard (updates daily): https://algoindex.com/tools/gamma-level-accuracy
- Research study built on this data: https://algoindex.com/research/expected-move-break-rate-spx-gamma-study
- Methodology notes: published on the scoreboard page

## Files

- `gamma-level-outcomes.csv` - one row per (date, underlying, expiry window, level type)

## Schema

| Column | Meaning |
|---|---|
| `date_et` | Trading date (US Eastern) |
| `underlying` | SPX, ES, or SPY view of the same level set (SPX is canonical; ES and SPY are translations) |
| `exp_window` | Which option-expiry set produced the level: `le45dte` (45 days or less), `0dte` (same-day), or `intraday` (session reference levels) |
| `level_type` | `call_wall`, `put_wall`, `gamma_flip`, `expected_range`, `key_gamma`, `vpoc`, `vah`, `val`, `pdh`, `pdl`, `onh`, `onl` |
| `level_price` | The marked level (empty for `expected_range`, which is a band) |
| `session_high` / `session_low` / `session_close` | The completed session's prints |
| `tested` | Whether price came within the tested threshold of the level that session |
| `outcome` | `held`, `broke`, or `not_tested` |
| `distance_pct` | Signed close-to-level distance as a fraction of the level |

## Grading rules (summary)

A level is tested when price comes within 0.25 percent of it. It is graded held when price tests it and closes on the expected side, broke when price closes decisively through it. Untested days are excluded from hit-rate denominators. The expected move band is graded on the close relative to the band edges.

## Important notes for analysis

- Use one underlying view (SPX recommended) to avoid triple-counting: ES and SPY rows describe the same levels.
- The record starts 2026-06-03 and grows every US trading session.
- Sample sizes are small early in the record. Always report the tested count next to any hit rate.

## License and citation

Data is released under CC BY 4.0. You are free to use it in articles, research, and tools with attribution:

> Data: AlgoIndex Gamma Level Accuracy Tracker, https://algoindex.com/tools/gamma-level-accuracy

## Refresh cadence

The CSV is refreshed from the live scoreboard periodically. For always-current numbers, use the live page or the export endpoint linked from it.
