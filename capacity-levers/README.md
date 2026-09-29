# Capacity levers: reserve, pause, scale and overage

Three Microsoft Fabric capacity scaling decisions, costed side by side: pay-as-you-go
against a 1-year reservation, scaling up against paying the overage or pausing, and
scaling against paying the overage for interactive load. No build step, no install, works
offline — open `capacity-levers.html` in a browser.

![Capacity levers](media/screenshot.png)

## Running it

No build step, no install, works offline. Just open `capacity-levers.html` in a browser.
Draws its own charts as inline SVG — no chart library, no CDN dependency.

## What it does

Costs three independent scaling "levers":

1. **Base capacity, plus peak for N days** — a base tier reserved all month, topped up
   with pay-as-you-go for the days that need more, against reserving the peak tier
   outright. Live inputs: base capacity, peak tier, days needing peak capacity.
2. **Scale for background, pay the overage, or pause** — a burst above a steady tier,
   costed three ways. Scaling up and reverting doesn't dodge the 24-hour smoothing tail,
   which still clears against the reverted ceiling at 3× the PAYG rate; pausing settles
   the same debt at once, at 1×, but cancels the running job. Live inputs: steady tier,
   burst usage %, burst duration, background floor %.
3. **Scale for interactive, or pay the overage** — 5-minute smoothing is effectively
   instantaneous, so duration cancels out of the comparison entirely: the crossover is a
   flat 133% burst height for a single-tier jump. Same four live inputs.

## Assumptions & caveats

- An **illustrative model**, not an official Microsoft pricing tool.
- Capacity overage is a **public preview** feature — its 3× PAYG rate isn't guaranteed and
  should be re-confirmed before relying on it. It lives in the tool as a single named
  `OVERAGE_MULTIPLIER` constant.
- Prices: North Europe, EUR, capacity checked against the Azure Retail Prices API on
  11 Sep 2026. Re-check before quoting a customer.
- Lever 2's breakeven is sensitive to the background floor: roughly 20 hours at the 80%
  default, 18 at 90%, and no breakeven at all below 50%, where paying the overage wins at
  every duration. Read it off the tool at stated inputs; don't quote it as a fixed formula.
