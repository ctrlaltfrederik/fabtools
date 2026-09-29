# Fabric Capacity Advisor

Sizes a new Microsoft Fabric project end to end and recommends an F-SKU, with a full
licensing and total-cost-of-ownership breakdown.

![Fabric Capacity Advisor](media/screenshot.png)

## Running it

No build step, no install, works offline. Just open `CapacityAdvisor.html` in a browser.

## What it does

- Pick a **project tier** (S/M/L/XL) and **workload archetype** (analytics / Power BI only /
  real-time / AI-augmented), tune the Power BI audience and a handful of real multipliers
  (refreshes/day, notebook runs, Dataflow Gen2 usage, paginated reports, ...) — it recommends
  the smallest F-SKU that fits.
- Choose **PAYG or 1-year reserved pricing up front** — the two bases don't always recommend
  the same SKU, so the pricing mode drives the recommendation from the start, not just the
  display.
- Runs the three **Power BI licensing paths** (Pro for all / F64+ free viewers / PPU for all)
  side by side and flags the cheapest viable one, including the viewer count where the
  cheapest option flips.
- Projects **total cost over time** — Production + Dev/Test, with an overridable Dev/Test
  tier and a "pause outside working hours" PAYG option — with a CSV export for finance.
- A "how it's calculated" page inside the tool documents every formula and coefficient behind
  the recommendation, so nothing is a black box.

## Assumptions & caveats

- An **illustrative model**, not an official Microsoft pricing tool.
- Prices: North Europe, EUR, checked against the Azure Retail Prices API and Microsoft's
  Power BI pricing page on 11 Sep 2026. Re-check before quoting a customer — the in-app
  "prices checked" note carries the same warning.
- Spark on-demand/autoscale billing is not modeled (a known gap, not a pricing error).

## About CapacityAdvisor.html

`CapacityAdvisor.html` is fully self-contained: it vendors React/ReactDOM in its own
`<script>` tags and the compiled component is spliced in as plain JS, so it never loads
anything else at runtime. The JSX source isn't published in this repo; the `.html` was
produced from it by:

1. Stripping the `import` line and the `export default` keyword from the JSX source.
2. Compiling what's left through Babel (`@babel/preset-env` + `@babel/preset-react`,
   `runtime: "classic"` so it emits `React.createElement` calls against the global `React`/
   `ReactDOM` the `.html` already loads, not the automatic JSX runtime).
3. Wrapping the compiled output in a fixed IIFE (`var useMemo = React.useMemo; ... try { ... }
   catch (e) { /* renders e into the page's own #errbox */ }`) and splicing it into the `.html`
   between the existing React/ReactDOM vendor `<script>` tags, leaving everything else
   untouched.
