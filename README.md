# India's Commodity-Driven Trade Deficit

How much of India's trade deficit is commodities — and what, if anything, pulls it back? This repo isolates the commodity leg of India's merchandise trade gap using the Commerce Ministry's own TRADESTAT (DGCI&S) data, traces its impact on the rupee, and stress-tests the one proven counterforce: metal recycling.

Companion repo: [india-trade-data-analysis](https://github.com/herrrickshaw/india-trade-data-analysis) (full 98-chapter trade, currency, inflation and macro datasets this work builds on).

## The headline numbers (FY2025-26, provisional)

| | Value | Share |
|---|---|---|
| Total merchandise imports | $776.0 bn | — |
| Merchandise trade deficit | −$334.3 bn | widened from −$184 bn in FY2018-19 |
| Commodity imports (oil + ch.71 + metal chapters) | $372.3 bn | **48% of all imports** |
| Bare gold alone (HS 7108) | $72.0 bn | 9.3% of all imports — 2nd only to crude |
| Silver (HS 7106) | $12.1 bn | tripled in one year |
| Copper complex (2603+7402+7403+7404) | $14.7 bn | compounding ~13%/yr for 8 years |
| Imported recycling feedstock (Cu+Al scrap) | $8.0 bn | India imports other countries' waste |

Commodity imports have been 46–53% of India's import bill in every one of the last eight fiscal years — the structural half of the deficit.

## What's in this repo

### `data/`

| File | Contents |
|---|---|
| [`tradestat_hsn_export_import_2018-19_to_2025-26.json`](data/tradestat_hsn_export_import_2018-19_to_2025-26.json) | All 98 HS chapters, export + import, 8 fiscal years, US$ million — the base layer |
| [`metals_hsn4_import_2024-25_to_2025-26.json`](data/metals_hsn4_import_2024-25_to_2025-26.json) | 4-digit bare-metal detail with full partner-country breakdowns: gold 7108, silver 7106, PGM 7110, diamonds 7102, copper complex 2603/7402/7403/7404, aluminium 7601/7602, Ni/Pb/Zn/Sn. Splits bullion out of chapter 71 — the 2-digit data can't |
| [`metal_recyclers_business_model_validation.json`](data/metal_recyclers_business_model_validation.json) | Business-model validation of India's metal recyclers (5 listed with TTM financials, plus the private battery/e-waste layer) against this trade data |

### `blog/`

[`blog_metal_recycling_import_bill.html`](blog/blog_metal_recycling_import_bill.html) — publish-ready post: *"India's Metal Import Bill Is $120 Billion. Recycling Is Already Eating Into It — Just Not Where You'd Expect."* Self-contained HTML, light theme, paste-into-Blogger ready.

## The three findings

### 1. The deficit's commodity core is mutating, not shrinking

Oil sets the floor (~$200 bn/yr, price-driven), but the FY2025-26 marginal pressure came from **precious metals**: chapter 71 supplied 37% of the year's entire import increase while oil *fell* $15 bn. The arbitrage channels actively migrate as customs closes each one — the platinum-group line collapsed −92% ($5.4 bn → $0.4 bn) when the "platinum alloy" route was shut, and the flow snapped back into bare gold; sourcing simultaneously shifted to doré from mining countries (Ghana +449%, Chile +2,486% YoY) for the refining-duty differential.

### 2. The rupee's slide is commodity-flow pressure, not inflation

The rupee's ~12% depreciation over the past year (₹86 → ₹96.4) happened while India's inflation (0.93%) ran *below* the US's (~2.7%) — PPP alone predicted strengthening. The realized depreciation is essentially all real: a widening goods deficit whose marginal driver was bullion, partly self-reinforcing (rupee weakness → gold buying → wider deficit). The services surplus ($189 bn) and rising reserves ($698 bn) explain why it's a slide, not a crisis.

### 3. Recycling is the one proven counterforce — and it works exactly where reverse logistics exist

**Lead is the existence proof**: chapter 78 is the only metal chapter where India's exports ($1.27 bn) exceed imports ($1.17 bn), because lead-acid batteries have India's only mature collection chain. Every profitable listed recycler is lead-first — Jain Resource Recycling (₹9,543 cr TTM revenue, +76%, 29% ROE on a 3.4% margin), Gravita, Pondy Oxides, Nile. The growth runway is the **$8 bn/yr of scrap India currently imports as feedstock** — domestic EoL collection (EPR, the ₹1,500 cr NCMM recycling scheme, ELV rules) substitutes those imports directly, no new technology required. Solar PV recycling (silver, silicon) is the 2028+ leg: loss-making today per CEEW (₹10–12k/tonne), flipped positive by EPR certificate trading and high-silver TOPCon waste.

## Method notes

- TRADESTAT is a Laravel/Livewire app whose report URLs 405 on a cold GET; all pulls use a session-cookie + CSRF-token workaround (documented inside each JSON's `method` field). Each query returns two fiscal years.
- FY2025-26 figures are provisional (source last updated May 2026); 4-digit pulls retrieved 3 Aug 2026.
- Company financials are yfinance TTM snapshots (3 Aug 2026); unlisted-company claims are descriptive and flagged as unverifiable.
- Nothing here is investment advice.
