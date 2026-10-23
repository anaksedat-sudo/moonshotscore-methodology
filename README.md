# MoonshotScore Methodology

The public specification of **MoonshotScore**, the 0–100 rating that [Stock Expert AI](https://www.stockexpertai.com) publishes for US-listed stocks.

This repository documents how the score is built, how it is banded, what data it uses and what it cannot tell you. It contains documentation only — no vendor market data and no company-level scores.

- Live methodology: https://www.stockexpertai.com/methodology
- Technical appendix (generated from the engine constants): https://www.stockexpertai.com/moonshot-methodology#technical-appendix
- Pricing and free-tier limits: https://www.stockexpertai.com/pricing
- Open dataset (CC BY 4.0): https://doi.org/10.5281/zenodo.23274464

> This is educational content, not investment advice. Past performance does not guarantee future results.

## What MoonshotScore is

MoonshotScore rates an eligible US-listed company from 0 to 100 using five **sector-relative pillars**. A higher score means stronger numbers across business quality, financial safety, valuation, growth durability and momentum, relative to peers. It is an analytical summary of reported data, not a forecast and not a probability of future returns.

Current model: **MoonshotScore V2** (engine constants versioned as v3.4 in the technical appendix). The retired V1 nine-signal model is documented in [METHODOLOGY.md](METHODOLOGY.md#historical-v1-model-retired) for archived records only.

## The five pillars

| Pillar | Question it answers | Weight | Share when all five have data |
|---|---|---|---|
| Business Quality | Is this a genuinely good business? | 26 | 28.3% |
| Financial Safety | Could this blow up? | 20 | 21.7% |
| Valuation | Am I paying a fair price? | 18 | 19.6% |
| Growth Durability | Is the growth real and likely to last? | 16 | 17.4% |
| Momentum | Is the market already moving on this? | 12 | 13.0% |

Weights sum to 92. The composite divides by the weights of the pillars actually present, so a pillar without data drops out and the rest are rescaled; it is never counted as zero. The input metrics of each pillar are listed in [METHODOLOGY.md](METHODOLOGY.md).

## How the score is computed (summary)

1. **Peer comparison.** Each input metric is compared with sector peers as a robust z-score (median and MAD, clipped at ±3). Sectors with fewer than 8 reporting companies are pooled.
2. **Pillars.** A pillar is the average of its available metric scores.
3. **Guards.** A positive Valuation reading is halved when ROIC or free-cash-flow yield is negative (value-trap guard); a positive Growth reading is cut to 40% when net margin is negative.
4. **Fragility penalties.** Altman Z distress/grey zone (outside financials and real estate), profit not backed by operating cash flow, and losses combined with negative retained earnings or cash burn are subtracted from the composite.
5. **Rank and shrink.** The adjusted composite becomes a percentile across the scored universe, then is drawn toward 50:
   `final = 50 + (percentile − 50) × √(coverage × size_factor)`.
   Thinly covered or very small companies therefore land closer to 50.

Full rules, caps and penalty formulas: [METHODOLOGY.md](METHODOLOGY.md).

## Score bands

| Band | Range |
|---|---|
| Exceptional | 80–100 |
| Strong | 65–79 |
| Fair | 45–64 |
| Weak | below 45 |

Bands do not hold a fixed share of companies, because scores are adjusted after ranking. The five pillar percentiles shown on stock pages explain the headline; they are not averaged into it.

## Dual-Channel principle: numbers from data, words from AI

Every figure on Stock Expert AI comes from licensed market data and is rendered as received. AI models write the explanatory text only; they never produce or alter a number. MoonshotScore itself is computed by deterministic code, not by a language model.

- Primary data: Financial Modeling Prep (FMP).
- Fallbacks only when FMP has no figure: Yahoo Finance (price or fundamental), Alpaca (price).
- SEC EDGAR: filing links and company identity only, never figures.
- News: page context only; news does not feed the V2 score.

## Universe

Common stocks and ordinary ADRs on NASDAQ, NYSE and AMEX with a positive reported market capitalization. OTC listings, funds, ETFs, warrants, units, SPACs, preferred shares and notes carry no score. A company without a usable V2 result has no current score; an archived V1 value is never substituted.

## Update cadence

Scores are recalculated from the most recent completed US trading session, so they move on US trading days. Each score carries its computation date; a newer price does not mean the score has refreshed, and a failed update leaves the previous dated score in place.

## Limitations

- MoonshotScore summarizes reported data; it does not predict prices or returns.
- Figures are taken from the provider as received and are not re-verified against a second source.
- Small and thinly covered companies are pulled toward 50 by design; size still influences the distribution (see the open dataset).
- The only forward check published so far is a single one-month window ([BACKTEST.md](BACKTEST.md)); it is not evidence of persistent outperformance.

## Open data

- *MoonshotScore snapshot of 3,638 US-listed stocks (26 Sep 2026) and a one-month score-band test* — Zenodo, CC BY 4.0, https://doi.org/10.5281/zenodo.23274464
- *Aggregate frequencies and coverage counts for a historical MoonshotScore snapshot (29 August 2026)* — Zenodo, CC BY 4.0, https://doi.org/10.5281/zenodo.22728643

Both releases publish derived scores and aggregates only, not raw vendor data.

## Citing

See [CITATION.cff](CITATION.cff). Suggested form: *Stock Expert AI (2026). MoonshotScore Methodology. https://www.stockexpertai.com/methodology*

## Corrections

Report errors to sedat@stockexpertai.com. Corrections to the methodology are logged on the site.

## License

Documentation in this repository is licensed under [CC BY 4.0](LICENSE). "MoonshotScore" and "Stock Expert AI" are names of Stock Expert AI.

---

This is educational content, not investment advice. Past performance does not guarantee future results.
