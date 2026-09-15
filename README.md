# Fabric capacity strategies — interactive tools

Two standalone, dependency-light HTML tools built alongside "The ultimate decision guide:
capacity strategies to optimise cost and compute" (delaware, FabCon Barcelona / dataMinds
Mechelen 2026). 

## Running them

No build step, no install. Just open the `.html` file in a browser.

- `licensing-crossover.html` draws its own bars in plain HTML/CSS — works fully offline.
- `pause-vs-reserve.html` loads [Chart.js](https://www.chartjs.org/) from a CDN
  (`cdnjs.cloudflare.com`) for its six charts, so it needs an internet connection once to
  render (everything else — the math, the sliders — runs locally, nothing is sent anywhere).

## The tools

### `licensing-crossover.html` — Pro vs PPU vs F64 licensing crossover

Costs three licensing strategies side by side for a given Power BI workload — **A: Pro for
all**, **B: F64+ with free viewers**, **C: PPU for all** — against a chosen Fabric candidate
SKU, model size, viewer/creator counts, capacity pricing mode (reserved vs PAYG), and whether
the org already has E5 (which folds in Pro at no extra cost). Live inputs: candidate SKU,
model size (GB), capacity pricing mode, E5 checkbox, viewer count, creator count. Highlights
the cheapest viable option and the viewer count where strategy A stops being the winner
against B.


## Assumptions & caveats (read before quoting numbers from either tool)

- Both are **illustrative models**, not published Microsoft formulas — the 24h/5-minute
  smoothing curves, the 70%/80% off-hours and background load defaults (now user-adjustable
  in `pause-vs-reserve.html`), and the "never clears" / breakeven conclusions are the point,
  not the exact CU figures.
- Capacity overage is a **public preview** feature; its 3× PAYG rate is not guaranteed and is
  flagged in both tools for re-confirmation before each delivery.
- Prices: North Europe, EUR, capacity checked against the Azure Retail Prices API on
  11 Sep 2026; Power BI Pro/PPU per-user prices checked against microsoft.com/nl-be on the
  same date, ex-VAT. Re-check both before relying on them — capacity and per-user pricing
  are not on the same refresh cycle.
- `pause-vs-reserve.html` answers a different question from the deck's own intraday slides
  (52/53, driven by `intraday-background.html` / `intraday-interactive.html`): those model a
  steady-state mismatch, this one models a scale-up-and-revert burst. Don't merge the two
  sets of numbers on stage.

## Attribution

Built by Frederik Declerck - delaware BeLux. Not official Microsoft guidance or a live pricing tool — each file's own
footer carries the full disclaimer.
