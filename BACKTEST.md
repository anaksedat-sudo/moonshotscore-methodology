# One-month score-band check (31 Aug – 30 Sep 2026)

A single, dated, descriptive check of how MoonshotScore V2 bands behaved over one month. It is published with its data on Zenodo: https://doi.org/10.5281/zenodo.23274464 (file `score-band-stress-test-2026-08-31_2026-09-30.csv`, CC BY 4.0).

> This is educational content, not investment advice. Past performance does not guarantee future results.

## Setup

- **Scores:** point-in-time V2 scores captured on 29 Aug 2026, not recalculated afterwards.
- **Universe:** 633 analysed US common stocks and ADRs.
- **Window:** entry at the 31 Aug 2026 close, exit at the 30 Sep 2026 close (price returns only).
- **Benchmark:** SPY, which returned −0.58% over the same window.
- **Missing prices** were left missing, never counted as zero.

## Results by band

| Band | n | Median return | Median excess vs SPY | Mean excess vs SPY | Share beating SPY | Share with drawdown > 20% |
|---|---|---|---|---|---|---|
| Exceptional | 114 | −3.21% | −2.63% | −2.70% | 31.6% | 1.8% |
| Strong | 134 | −3.78% | −3.20% | −3.20% | 28.4% | 6.7% |
| Fair | 153 | −5.19% | −4.62% | −4.29% | 36.0% | 9.2% |
| Weak | 232 | −7.13% | −6.56% | −5.97% | 34.5% | 24.6% |

## What it shows (and does not)

- Median excess return fell band by band, from Exceptional to Weak.
- The clearest separation was downside risk: 24.6% of Weak-band stocks had a drawdown over 20%, against 1.8% of Exceptional-band stocks.
- The share of stocks beating SPY was **not** ordered by band.
- All four bands trailed SPY in this window.
- This is one 30-day window in one market regime. It is not evidence of persistent outperformance, and the site's official performance record (https://www.stockexpertai.com/score-track-record) follows its own protocol.

---

This is educational content, not investment advice. Past performance does not guarantee future results.
