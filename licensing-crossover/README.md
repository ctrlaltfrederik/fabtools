# Licensing crossover: Pro vs PPU vs F64

Costs three Power BI licensing strategies side by side for a given workload and finds the
viewer count where the cheapest option changes.

![Licensing crossover](media/screenshot.png)

## Running it

No build step, no install, works offline. Just open `licensing-crossover.html` in a browser.

## What it does

Compares three licensing paths against a chosen Fabric candidate SKU and model size:

- **A — Pro for all**: every viewer and creator on a Power BI Pro license.
- **B — F64+, free viewers**: capacity raised to F64 or above, viewers ride free on the
  capacity, only creators need Pro.
- **C — PPU for all**: every viewer and creator on Premium Per User.

Live inputs: candidate SKU, model size (GB), capacity pricing mode (reserved vs PAYG), an "org
already has E5" checkbox (folds Pro in at no extra cost), viewer count, creator count.
Highlights the cheapest viable option and the viewer count where Option A stops winning
against Option B.

## Assumptions & caveats

- An **illustrative model**, not an official Microsoft pricing tool — the crossover math is
  the point, not the exact euro figures.
- Prices: North Europe, EUR, capacity checked against the Azure Retail Prices API on
  11 Sep 2026; Power BI Pro/PPU per-user prices checked against microsoft.com/nl-be on the
  same date, ex-VAT.
