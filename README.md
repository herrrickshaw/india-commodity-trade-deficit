# India's commodity-driven trade deficit

How much of India's merchandise trade deficit is commodities, and what pulls it back? This repo
isolates the commodity leg of India's trade gap from the Commerce Ministry's own TRADESTAT
(DGCI&S) data — 98 HS chapters over eight fiscal years plus 4-digit bare-metal import detail by
partner country — and stress-tests the one proven counterforce, metal recycling, against the listed
recyclers' financials. The outputs are three JSON datasets in `data/` and one publish-ready blog
post in `blog/`. Domain: Indian merchandise trade, FY2018-19 to FY2025-26.

> Status: research-only, data-and-findings snapshot. No code lives here; the datasets were pulled
> by hand-driven HTTP sessions documented in each JSON's `method` field. FY2025-26 figures are
> provisional. Nothing here is investment advice.

Companion repo: [india-trade-data-analysis](https://github.com/herrrickshaw/india-trade-data-analysis)
(full 98-chapter trade, currency, inflation and macro datasets this work builds on).

## The headline numbers (FY2025-26, provisional)

All figures recomputed from `data/` on 2026-11-02; US$ bn.

| | Value | Share |
|---|---|---|
| Total merchandise imports | $776.0 bn | — |
| Merchandise trade deficit | −$334.3 bn | widened from −$184.0 bn in FY2018-19 |
| Commodity imports (ch. 26, 27, 71, 72–81) | $372.3 bn | **48% of all imports** |
| Bare gold alone (HS 7108) | $72.0 bn | 9.3% of all imports — second only to crude |
| Silver (HS 7106) | $12.1 bn | 2.5× the prior year ($4.8 bn) |
| Copper complex (2603 + 7402 + 7403 + 7404) | $14.7 bn | — |
| Imported recycling feedstock (Cu scrap 7404 + Al scrap 7602) | $8.0 bn | India imports other countries' waste |

Commodity imports have been 46–53% of India's import bill in every one of the last eight fiscal
years — the structural half of the deficit.

## What it does
- `tradestat_hsn_export_import_2018-19_to_2025-26.json`: export and import value for every
  populated HS chapter (98 of 99), eight fiscal years, US$ million, with per-year totals.
- `metals_hsn4_import_2024-25_to_2025-26.json`: 4-digit import detail with full partner-country
  breakdowns for gold 7108, silver 7106, PGM 7110, diamonds 7102, copper 2603/7402/7403/7404,
  aluminium 7601/7602 and Ni/Pb/Zn/Sn — the split of bullion out of chapter 71 that 2-digit data
  cannot give.
- `metal_recyclers_business_model_validation.json`: five listed Indian metal recyclers with TTM
  financials, the private battery/e-waste layer, CEEW solar-PV recycling economics, and sector
  findings tested against the trade data above.
- `blog/blog_metal_recycling_import_bill.html`: self-contained, light-theme post ready to paste into
  Blogger — *"India's Metal Import Bill Is $120 Billion. Recycling Is Already Eating Into It — Just
  Not Where You'd Expect."*

## Quick start
```bash
git clone https://github.com/herrrickshaw/india-commodity-trade-deficit.git
cd india-commodity-trade-deficit
python3 -c "import json; d=json.load(open('data/tradestat_hsn_export_import_2018-19_to_2025-26.json')); print(d['totals'])"
```
There is nothing to install. Open the blog post in a browser; load the JSON files with any JSON
reader.

## How it works
| Stage | Source / method | Output |
|---|---|---|
| Chapter series | TRADESTAT Commodity-wise Export / Import reports, 2-digit, 4 POSTs per direction (each returns two fiscal years), retrieved 2026-07-18 | `tradestat_hsn_export_import_…json` |
| Bare-metal detail | TRADESTAT Commodity-wise All Countries Import report, 4-digit, retrieved 2026-08-03 | `metals_hsn4_import_…json` |
| Recycler validation | yfinance `.info` TTM snapshots (2026-08-03) + CEEW + policy documents, read against the two datasets above | `metal_recyclers_business_model_validation.json` |
| Write-up | hand-written HTML from the three JSONs | `blog/…html` |

TRADESTAT is a Laravel/Livewire app whose report URLs return 405 on a cold GET. Every pull first
visits the search page to obtain a session cookie and CSRF `_token`, then POSTs the form; the exact
parameters are recorded in each JSON's `method` field so the pull is reproducible.

## The three findings

### 1. The deficit's commodity core is mutating, not shrinking
Oil sets the floor (~$200 bn/yr, price-driven), but the FY2025-26 marginal pressure came from
precious metals: chapter 71 supplied 37% of the year's entire import increase while oil *fell*
$15 bn. Arbitrage channels migrate as customs closes each one — the platinum-group line (7110)
collapsed −92% ($5.4 bn → $0.4 bn) when the "platinum alloy" route was shut, and the flow snapped
back into bare gold; gold sourcing simultaneously shifted to doré from mining countries (Ghana
+449%, Chile +2,486% YoY) for the refining-duty differential.

### 2. The rupee's slide is commodity-flow pressure, not inflation
The rupee's ~12% depreciation over the past year (₹86 → ₹96.4) happened while India's inflation ran
below the US's, so purchasing-power parity alone predicted strengthening. The realised depreciation
is essentially all real: a widening goods deficit whose marginal driver was bullion, partly
self-reinforcing (rupee weakness → gold buying → wider deficit). The services surplus and rising
reserves explain why it is a slide, not a crisis. *The currency, inflation and balance-of-payments
inputs for this finding live in the companion repo, not in `data/` here.*

### 3. Recycling is the one proven counterforce, and it works exactly where reverse logistics exist
Lead is the existence proof: chapter 78 is the only metal chapter where India's exports ($1.27 bn)
exceed imports ($1.17 bn), because lead-acid batteries have India's only mature collection chain.
Every profitable listed recycler is lead-first — Jain Resource Recycling (₹9,543 cr TTM revenue),
Gravita, Pondy Oxides, Nile. The growth runway is the $8 bn/yr of scrap India currently imports as
feedstock: domestic end-of-life collection (EPR, the ₹1,500 cr NCMM recycling scheme, ELV rules)
substitutes those imports directly with no new technology. Solar-PV recycling (silver, silicon) is
the 2028+ leg: loss-making today per CEEW, flipped positive by EPR certificate trading and
high-silver TOPCon waste.

## Repository layout
| Path | Purpose |
|---|---|
| `data/tradestat_hsn_export_import_2018-19_to_2025-26.json` | Base layer: 98 chapters × 8 FYs × export/import, US$ million |
| `data/metals_hsn4_import_2024-25_to_2025-26.json` | 4-digit bare-metal imports by partner country, FY2024-25 and FY2025-26 |
| `data/metal_recyclers_business_model_validation.json` | Recycler financials + sector findings |
| `blog/blog_metal_recycling_import_bill.html` | Publish-ready post |
| `HOW_TO_USE_WITH_AI.md` | Generic guide for non-programmers: hand the repo to an AI assistant |
| `CLAUDE.md` | Conventions for Claude Code sessions (source-fetching rules) |
| `.github/workflows/quality-gates.yml` | gitleaks secret scan on every push / PR |
| `graphify-out/` | Generated knowledge-graph cache; not hand-edited |

All files in `data/` are tracked and hand-pulled; nothing is generated at build time.

## Data sources
| Source | What is taken | Access route | Licence / caveat |
|---|---|---|---|
| [TRADESTAT / DGCI&S](https://tradestat.commerce.gov.in/eidb) | Commodity-wise export/import by HS chapter and HS4 by country | Session-cookie + CSRF-token POST (scraped; see `method` fields) | Government of India data; FY2025-26 provisional, source last updated 19 May 2026 |
| yfinance | TTM financials for five listed recyclers | `.info` snapshots, 2026-08-03 | Point-in-time; not refreshed |
| CEEW, "How Much Does it Cost to Recycle a Solar Module in India?" (Kale & Tyagi, Nov 2025) | PV recycling cost economics | Read, cited in JSON | Author's reading |
| DST and Ministry of Mines (NCMM) policy documents | Recycling incentive scheme details | Read, cited in JSON | As published |

## Limitations and known issues
- FY2025-26 is provisional throughout; TRADESTAT revises figures after publication.
- TRADESTAT returns 405 on a plain GET and needs a fresh `_token` per request (reported elsewhere
  as a 419 on reuse); there is no script here — re-pulls must repeat the documented manual session.
- Company financials are yfinance snapshots from 2026-08-03 and will be stale; unlisted-company
  claims are descriptive and flagged as unverifiable inside the JSON.
- Finding 2 depends on currency and inflation series that are not in this repo.
- "Commodity imports" is defined as chapters 26, 27, 71 and 72–81; other definitions give
  different shares.

## Related repositories
- [india-trade-data-analysis](https://github.com/herrrickshaw/india-trade-data-analysis) — the
  full trade, currency, inflation and macro datasets this repo builds on.

## Licence
No licence file yet — all rights reserved.
