# MoonshotScore V2 — Specification

Source of truth: the live pages https://www.stockexpertai.com/methodology (doc version 1.0, last updated 2026-09-28) and the technical appendix https://www.stockexpertai.com/moonshot-methodology#technical-appendix (engine constants v3.4). If this file and the live pages ever differ, the live pages win; please open an issue.

> This is educational content, not investment advice. Past performance does not guarantee future results.

## 1. Scored universe

- Listing on NASDAQ, NYSE or AMEX.
- Active common equity or ordinary ADR, plus unclassified listings with no evidence of being a non-equity instrument; US-listed foreign companies are included.
- Positive reported market capitalization.
- Excluded: OTC listings, funds, ETFs, warrants, units, SPACs, preferred shares, notes and other debt instruments.

## 2. Factor inputs by pillar

| Pillar | Weight | Inputs |
|---|---|---|
| Business Quality | 26 | ROIC, ROE, ROA, gross profit/assets, gross margin, operating margin, net margin, FCF yield |
| Financial Safety | 20 | Altman Z, Piotroski score, net debt/EBITDA, debt/equity, interest cover, current ratio, beta |
| Valuation | 18 | earnings yield, FCF yield, inverse EV/EBITDA, inverse EV/FCF, inverse EV/sales, inverse price/book, dividend yield |
| Growth Durability | 16 | revenue growth, 3-year and 5-year revenue/share growth, EPS growth, operating-income growth, FCF growth, 5-year net-income/share growth, dilution (lower is better) |
| Momentum | 12 | 12-month, 6-month and 3-month price change, price / 52-week high |

Financial Services omits enterprise-value multiples, Altman Z, net debt/EBITDA, debt/equity, interest cover, current ratio and gross profit/assets.

## 3. Transformations

| Stage | Rule |
|---|---|
| Peer groups | Compare within sector when at least 8 companies report a factor; otherwise pool the small sectors. |
| Outliers | With at least 5 readings, winsorize at the 2nd and 98th percentile. |
| Standardization | Robust z = 0.6745 × (value − median) ÷ MAD, clipped to ±3; lower-is-better factors reverse sign. |
| Input caps | ROIC and ROE ±0.6; revenue growth ±0.6; 3-year revenue/share ±0.5; 5-year revenue/share and net income/share ±0.4; EPS, operating-income and FCF growth ±1. |
| Ratios | Interest cover: signed log1p after a ±20 cap. Debt/equity: log1p after a 20 cap; when net debt/EBITDA is 0–1, a debt/equity reading above 1 is omitted. EV/EBITDA, EV/FCF, EV/sales and price/book are inverted subject to the negative-value rules. |
| Pillars | Average of available factor z-scores. |
| Guards | Positive Valuation × 0.5 after a negative ROIC or FCF yield; positive Growth × 0.4 after a negative net margin. |
| Composite | Weighted average of the pillars present (weights 26/20/18/16/12, rescaled over present pillars). |
| Rank and shrink | Rank the fragility-adjusted composite (ties share the average rank: 100 × (values below + ties ÷ 2) ÷ N), then `50 + (percentile − 50) × √(coverage × size_factor)`. |
| Size factor | Ranges from 0.5 to 1.0 across log10 market cap 7.7 to 9.3 (about $50M to about $2B). |
| Published precision | Headline to 1 decimal; coverage and fragility to 2 decimals; displayed pillar percentiles to 0 decimals. |

## 4. Missing and negative values

- Blank, None and non-finite provider values become null, never zero.
- A missing metric is omitted from its pillar average; a pillar with no usable metrics is omitted and the remaining weights are rescaled.
- Coverage is the weighted share of usable factor inputs; lower coverage draws the final percentile toward 50.
- Negative debt/equity and negative net debt/EBITDA are treated as missing.
- EV/EBITDA, EV/FCF and price/book must be positive before inversion; zero or negative values are missing.
- A zero EV/sales multiple is missing; a negative EV/sales multiple remains a signed input.
- Negative earnings yield, FCF yield, margins and growth rates remain observations; they can lower peer ranks and activate the guard or fragility rules.

## 5. Subtractive fragility penalties (applied before ranking)

| Trigger | Amount subtracted |
|---|---|
| Altman Z below 1.1 (except Financial Services and Real Estate) | (1.8 − 1.1) × 0.2 + (1.1 − max(Z, −1)) × 0.9 |
| Altman Z from 1.1 to below 1.8 (same exceptions) | (1.8 − Z) × 0.2 |
| Positive net margin with income quality below 0.8 | (0.8 − max(income quality, −1)) × 0.6 |
| Negative retained earnings and negative net margin | 1.2 + min(\|net margin\|, 1) × 0.8 |
| Otherwise, negative net margin and negative FCF yield | 0.8 + min(\|net margin\|, 1) × 0.6 |

## 6. Mechanical worked example

In a two-company demonstration universe, the higher adjusted composite has zero-based rank index 1. Its raw percentile is 100 × (1 + 0.5) ÷ 2 = 75.0. With 100% coverage and a market cap at the size-ramp ceiling, the size factor is 1.0, so the final score is 50 + (75.0 − 50) × √(1.0 × 1.0) = 75.0. This demonstrates the mechanics; it is not a company score.

## 7. Bands

Exceptional 80–100 · Strong 65–79 · Fair 45–64 · Weak below 45. Bands do not contain a fixed share of companies.

## 8. Data sources

| Source | Role |
|---|---|
| Financial Modeling Prep (FMP) | Primary: quotes, market cap, fundamentals, statements, analyst ratings, insider transactions, technical indicators |
| Yahoo Finance | Fallback only, when FMP has no figure for a ticker |
| Alpaca Markets | Price fallback only, when neither FMP nor Yahoo has one |
| SEC EDGAR | Filing links and company identity only; no figures |
| News providers | Page context only; not a V2 input |

## 9. Update frequency

Recalculated from the most recent completed US trading session. Each score shows its computation date. A failed update keeps the previous dated score.

## Historical V1 model (retired)

Kept only to explain archived records; never used as a fallback for a missing V2 score. V1 combined nine signals with fixed weights: Revenue Growth 20%, Gross Margin 10%, Operating Leverage 10% (a proxy computed from revenue growth, so revenue growth effectively drove 30%), Cash Runway 10%, R&D Intensity 10%, Insider Activity 15%, Trading Liquidity 5%, Price Momentum 10%, News Sentiment 10%. V1 letter grades: A+ 80–100, A 70–79, B+ 60–69, B 50–59, C 40–49, D 30–39, F below 30.

---

This is educational content, not investment advice. Past performance does not guarantee future results.
