---
name: water-report-extraction
description: Use when you need hardness, alkalinity, calcium, magnesium, sodium, TDS or pH for a city from its utility's water quality report (Consumer Confidence Report, CCR), for a dataset or a tool built on it.
---

# Extracting water figures from a utility report

Goal: one verified record per water system, with units normalized and a page cite, so a reader can check every number.

## Steps

1. Find the newest report. Search "CITY water quality report" or "CITY consumer confidence report YEAR". Prefer the utility's own domain (.gov, .org). Record the URL and the report year.
2. Delegate the reading to `scout` (cheap). Ask for JSON written to a file, one object per city, with these fields: `status` (usable / blocked / no_usable_data), `report_year`, `source_url`, `pdf_page`, `total_hardness_mg_L_as_CaCO3`, `total_hardness_gpg`, `alkalinity`, `calcium`, `magnesium`, `sodium`, `tds`, `ph`, `zip_codes_or_areas`, `notes`. Tell it to use null for anything not printed, never to compute or guess, and to report HTTP 403s instead of working around them.
3. Check units yourself. Reports mix mg/L, ppm (same as mg/L for water), grains per gallon (1 gpg = 17.1 mg/L as CaCO3) and mmol/L. Calcium hardness as CaCO3 = Ca mg/L x 2.497; magnesium hardness = Mg mg/L x 4.116. Alkalinity must say "as CaCO3".
4. Watch for ranges and averages. Keep the range and say which one the tool shows. A plant average is not the water at one tap: record the zip codes or areas the plant serves, and note multi-source systems.
5. Spot-check against the PDF: open the page the scout cited (or fetch the text with `pdftotext -f N -l N file.pdf -`) and confirm at least hardness and one mineral. If the scout's page number is wrong or the value differs, fix it and spot-check the rest of that batch.
6. Add the record to the dataset with the cite (URL, page, year). Leave null values null.

## What goes wrong

- Blocked sites (HTTP 403): treat as a no. Try the utility's other pages or a different year; otherwise skip the city and say so.
- Data older than about three years: label the year, or skip.
- Scanned or Spanish-language PDFs may yield ranges only; accept ranges, don't invent averages.
- Hardness missing but calcium and magnesium present: compute it using the factors above and mark it "computed".

## Changes

- 2026-10-16: first version, from 21 city records built in sessions 51-82.
