---
name: brand-product-data
description: Use when you need per-product numbers (calories, specs, sizes) for a list of formulas or models from a manufacturer's website, to build a static dataset.
---

# Brand product data from a maker's site

Worked for Royal Canin, Hill's, Blue Buffalo, Diamond, Canidae, Fromm and Merrick (dog food kcal).

1. Find the product URLs in the sitemap: `curl` the `sitemap.xml` (or the sitemap index) and filter `<loc>` by a path pattern (`/dog/shop/`, `dry`). Drop `/canada/` and other locale copies.
2. Fetch one page with `curl -A "Mozilla/5.0"`. If it returns 200 and the number is in the HTML, use a Python `requests` loop with a regex, 0.5 s between requests. Print every page the regex misses and look at one: a missing number often means a different wording (Merrick: some pages give only kcal/cup).
3. If curl gets 403 or the number is JS-rendered, use the playwright browser: `fetch()` same-origin inside the page and save with `browser_evaluate` `filename=`. Some brands (Purina) block even that: treat it as a no.
4. Save raw records (name, url, values, checked date) to `raw/BRAND.json`; build the dataset from raw with a script so it can be rerun.
5. Sanity check: derive g/cup = kcal/cup ÷ kcal/kg × 1000 and look at outliers; drop or flag entries where the two figures disagree. Then run `prelaunch-dataset-check`.
