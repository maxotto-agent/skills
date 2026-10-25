---
name: manual-quote-extraction
description: Use when a dataset row must rest on a sentence in a maker's PDF manual (car-seat expiry, battery date codes, appliance serials): extract text with pypdf, keep one verbatim quote per row, spot-check against the PDF.
---

# Manual quote extraction

Used for the car-seat dataset (46 rows from Chicco, Evenflo and Britax manuals) and the battery rules.

1. Download each PDF with `curl -L -o` into a work folder. Record the URL and the download date per file. A 403 or a login wall means no; do not work around it. List the files you lack so the owner can supply them.
2. Extract text: `python3 -c "import pypdf,sys; r=pypdf.PdfReader(sys.argv[1]); print('\n'.join(p.extract_text() or '' for p in r.pages))" FILE.pdf > FILE.txt`. If the output is empty, the PDF is scanned; skip it and note it.
3. Search the text with a regex for the fact (`grep -n -i -E "expir|years|useful life"`). Pull the shortest sentence that states the fact. Whitespace is often broken by extraction; normalize spaces but never reword.
4. One row per model: `{brand, model, value, quote, source_url, page, retrieved}`. If the manual doesn't state the fact, set `value: null` and say so in `note`; never fill from memory or from another model's manual.
5. Deduplicate models that appear in several manuals. If duplicates disagree, keep neither until you've read both.
6. Spot-check: for at least 5 rows, or all rows if there are fewer than 20, reopen the PDF and confirm the quote is verbatim and the value matches. Log the check in the journal.
7. Rebuild any page that embeds the data from the JSON file, then run `prelaunch-dataset-check`.
