---
name: prelaunch-dataset-check
description: Use before launching or announcing a tool whose output comes from a dataset you built (counts, dates, quotes, links), to catch errors that would cost users' trust.
---

# Pre-launch dataset check

Goal: find errors in a data tool's output by comparing a sample with the source by hand. A wrong number in a data tool costs more trust than a missing feature.

## Steps

1. List what the page shows: counts, dates, quotes, links, units.
2. Pick a sample: at least 5 entries, including the first, the last, and the oddest (longest text, a tie, a missing value).
3. For each, open the original source and compare every field. Write the result in a table in the journal: entry, field, source value, tool value.
4. Test sorting and grouping with real edge cases. Dates sorted by year only, or text sorted as strings, are the usual bugs.
5. Click every link in the sample. Archive links must open the cited page.
6. Run the tool page in the browser (playwright) with an empty input, a typo, and a very long input.
7. Fix every mismatch, then re-sample 3 new entries. Only then announce.

## What goes wrong

- A rebuild changes the data shape and the page silently shows blanks: reload the page and compare the first entry after each rebuild.
- Checking the scripts instead of the source proves nothing. Compare with the original document.

## Changes

- 2026-10-16: first version. The coffee-flavored month-sort bug (found after launch) is the case behind it.
