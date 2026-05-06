# Wheat in a Warming World
### A data project tracking climate shocks, geopolitical disruptions, and speculative dynamics in global wheat markets

---

## Overview

Since 1990, global wheat prices have experienced several major spikes — in 2007-08, 2010-11, and 2022. Each was different in origin: a confluence of supply shocks and speculation in 2007-08; a Russian heatwave and cascading export restrictions in 2010-11; a land war in Europe in 2022. Yet in each case, the countries that paid the most were not the ones responsible for the disruption.

This project asks three questions:

1. **How do climate, geopolitical, and financial shocks propagate through global wheat supply chains?**
2. **How does the price of a global commodity spike translate — unevenly — into the cost of bread in food-insecure countries?**
3. **When prices spike beyond what supply fundamentals can explain, what role does financial speculation play?**

The analysis proceeds in four phases, each building on the last: trade and production flows, price transmission, shock classification, and speculative modelling.

---

## Why Wheat

Wheat provides 20% of global calories and protein for approximately 3.4 billion people. It is among the most geopolitically significant commodities on earth — a small number of countries dominate exports, and import dependency is highest in the regions least able to absorb price shocks.

It is also among the most climate-sensitive staple crops. Heat stress during the flowering window of as little as 2-3 days above 35°C causes irreversible yield loss. Drought, late frost, and flooding each affect different stages of the growing season. As the frequency and severity of these events increases, understanding their transmission into prices and food security becomes more urgent.

---

## Scope

### Time Period
1990 – 2024

### Country Selection

Countries are selected programmatically using a principled threshold, not arbitrary choice.

**Producers / Exporters** — countries that have accounted for >5% of global wheat exports in at least one year since 1990. This yields approximately 6-8 countries: Russia, United States, Canada, Australia, Ukraine, and key EU exporters (France, Germany). Climate, yield, and export data are analysed for this group.

**Importers** — selected on two criteria applied jointly:
- High wheat import dependency (wheat imports as share of domestic consumption, from FAO data)
- High vulnerability to price shocks (cross-referenced with FEWS NET and the Global Food Security Index)

Approximately 10-12 countries are included, drawn from MENA, Sub-Saharan Africa, and South Asia — the regions where global price spikes most directly translate into food insecurity. A small comparator group of wealthy importers (Japan, UK, Mexico) is included to contrast how the same global shocks are absorbed differently.

Full selection criteria and country lists are documented in `notebooks/00_scope.ipynb`.

---

## Project Structure

```
wheat-climate-shock/
│
├── data/
│   ├── raw/                  # Unmodified source data
│   └── processed/            # Cleaned, merged datasets
│
├── notebooks/
│   ├── 00_scope.ipynb        # Country selection methodology
│   │
│   ├── phase1/               # Trade & Production
│   │   ├── 01_production_yields.ipynb
│   │   ├── 02_export_import_flows.ipynb
│   │   └── 03_yield_trade_linkage.ipynb
│   │
│   ├── phase2/               # Price Transmission
│   │   ├── 04_global_prices.ipynb
│   │   ├── 05_local_flour_prices.ipynb
│   │   └── 06_price_transmission.ipynb
│   │
│   ├── phase3/               # Shock Classification
│   │   ├── 07_climate_stress.ipynb
│   │   ├── 08_geopolitical_events.ipynb
│   │   ├── 09_shock_taxonomy.ipynb
│   │   └── 10_shock_impact.ipynb
│   │
│   └── phase4/               # Speculative Model
│       ├── 11_stocks_to_use_model.ipynb
│       └── 12_cftc_speculation.ipynb
│
├── src/
│   ├── data_pipeline.py      # Data fetching and cleaning
│   ├── climate_utils.py      # ERA5 extraction, SPEI processing
│   └── shock_classifier.py   # Shock taxonomy logic
│
├── figures/                  # Publication-quality outputs
├── data_sources.md           # Full source documentation
└── README.md
```

---

## Analytical Phases

### Phase 1 — Trade & Production
*Who grows wheat, who trades it, and how do yield shocks move through supply chains?*

Starting from FAOSTAT production and trade data, this phase establishes yield anomalies for major producers and examines how those anomalies propagate into export and import behaviour. Key questions:
- How do yield anomalies affect the export behaviour of producing countries?
- How do yield shocks in major exporters affect import volumes in dependent countries?
- Does simultaneous stress across multiple exporters produce non-linear effects?

**Status:** In progress

---

### Phase 2 — Price Transmission
*How does a global price spike become a local food crisis?*

Global benchmark prices (World Bank Pink Sheet) are linked to local retail flour prices across the importer country group. The transmission mechanism — how quickly and completely global price changes pass through to domestic markets — varies significantly by country wealth, trade policy, and reserve capacity. Key questions:
- How quickly do local prices respond to global price changes?
- Which countries absorb shocks through subsidies or reserves, and which pass them directly to consumers?
- Which countries pay more when prices spike? Which benefit?

**Status:** Planned

---

### Phase 3 — Shock Classification
*What actually caused each price spike?*

A taxonomy of supply shocks is constructed and applied to historical price spike events. Shocks are classified by type and linked to their primary transmission channel:

```
Shock taxonomy
├── Climate                       → affects yield
│   ├── Heat stress (flowering window)
│   ├── Drought (SPEI index)
│   ├── Flood / excess rainfall
│   └── Late frost / freeze
├── Geopolitical                  → affects export availability
│   ├── War / conflict
│   └── Export restrictions
└── Financial                     → affects market price directly
    └── Speculative price amplification
```

Climate stress variables are seasonally anchored to each country's wheat growth calendar (USDA crop calendar), so that temperature and rainfall anomalies are measured during the specific growth windows where they cause yield damage — not as annual averages.

Key questions:
- Are climate shocks becoming more frequent or more severe over the study period?
- Do shocks cluster geographically? Are major exporters hit simultaneously more often over time?
- How much does each shock type contribute to global vs. local price changes?

**Status:** Planned

---

### Phase 4 — Speculative Model
*When prices spike beyond what fundamentals explain, what is the role of financial markets?*

A fundamentals-based price model is constructed using stocks-to-use ratios, production anomalies, energy prices, and exchange rates. Periods where actual prices significantly exceed model predictions are identified as candidate speculative episodes. These are cross-referenced with CFTC Commitments of Traders data, which distinguishes commercial hedgers (farmers, millers) from non-commercial speculators (hedge funds, index funds).

Key questions:
- Do large non-commercial futures positions correspond to periods of price-fundamentals divergence?
- Which shock types are associated with elevated speculative activity?
- Does speculation amplify real shocks, or generate independent price movements?

Note: This phase does not claim to prove causation. It identifies patterns consistent with speculative amplification and documents the methodology transparently.

**Status:** Planned

---

## Data Sources

| Dataset | Source | Used In |
|---|---|---|
| Wheat production, exports, imports | [FAOSTAT](https://www.fao.org/faostat/) | Phases 1, 2, 3 |
| Global wheat price (Pink Sheet) | [World Bank](https://www.worldbank.org/en/research/commodity-markets) | Phases 2, 3, 4 |
| Local retail flour prices | [FAO / World Bank](https://www.fao.org/giews/food-prices/) | Phase 2 |
| Wheat stocks-to-use ratio | [USDA WASDE](https://www.usda.gov/oce/commodity/wasde/) | Phase 4 |
| Temperature & precipitation (ERA5) | [Copernicus CDS](https://cds.climate.copernicus.eu/) | Phase 3 |
| Drought index (SPEI) | [CSIC Global SPEI Database](https://spei.csic.es/) | Phase 3 |
| Wheat crop calendars | [USDA / GGCMI](https://data.giss.nasa.gov/crops/) | Phase 3 |
| Armed conflict data | [UCDP/PRIO](https://ucdp.uu.se/) | Phase 3 |
| Export restriction events | Hand-coded from FAO/UNCTAD policy records | Phase 3 |
| Futures market positioning | [CFTC Commitments of Traders](https://www.cftc.gov/MarketReports/CommitmentsofTraders/) | Phase 4 |

Full source documentation, including variable definitions, download dates, and known limitations, is in `data_sources.md`.

---

## Methodological Notes

**Yield anomalies** are calculated as the deviation from a 10-year rolling mean, expressed in standard deviations. This controls for long-run productivity trends (improved seeds, fertiliser access) so that the anomaly reflects weather-driven variation rather than structural change.

**Climate stress variables** are measured during growth-stage-specific windows defined by the USDA crop calendar for each country, not as annual or seasonal averages. A heatwave in January is irrelevant to a Northern Hemisphere wheat crop; the same event in May during flowering is catastrophic.

**Compound climate events** — simultaneous heat stress and drought — are flagged separately, as the literature suggests their combined yield impact is non-linear.

**Speculative episodes** are identified using a two-step method: (1) statistical flagging of price spikes exceeding 1.5 standard deviations above a 24-month rolling mean, (2) cross-referencing with stocks-to-use ratios and CFTC positioning data. Classification is transparent and reproducible; causal claims are not made.

**Country selection** uses explicit, programmatic thresholds documented in `notebooks/00_scope.ipynb`. Selection criteria are applied consistently and do not reflect subjective judgement.

---

## Limitations

- ERA5 reanalysis data represents modelled estimates, not direct observations. Coverage and accuracy vary by region.
- Retail flour price data has significant gaps for some vulnerable importer countries, particularly in Sub-Saharan Africa.
- The conflict dataset codes conflict presence, not proximity to wheat-growing regions within a country. This is a known simplification.
- The speculative model is a reduced-form approach. It identifies candidate episodes rather than establishing causation.
- Country-level crop calendars mask within-country variation, particularly for large countries like Russia and the United States.

---

## Background Reading

- Lobell et al. (2011) — Climate trends and global crop production since 1980, *Science*
- Tadesse et al. (2014) — Drivers and triggers of international food price spikes and volatility, *Food Policy*
- Luo & Tanaka (2021) — Food import dependency and price volatility transmission, *Foods*
- IPCC AR6 (2022) — Chapter 5: Food, Fibre, and Other Ecosystem Products

---

## License

MIT License. Data sources are subject to their own terms of use — see `data_sources.md`.