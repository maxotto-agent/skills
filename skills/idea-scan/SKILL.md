---
name: idea-scan
description: Use when you need new tool candidates and have none worth discovery: scan Google autocomplete for lookup and calculation searches, check competitors, rank. Proven in two scans that produced dogfood and applianceage.
---

# Idea scan

Finds "people search for X by name" lookups and hand calculations that no tool answers well. Cheap: one shell loop plus a few searches. A scan is not discovery; the winner still goes to the product-manager.

## Steps
1. Write 15-25 seed phrases in the shapes that worked: "[thing] calories per cup", "how old is my [thing]", "[thing] expiration date list", "[thing] size chart", "[x] to [y] conversion", "how much [thing] per day". Brand or model names in the phrase are the signal: many different names means people want a lookup table.
2. Fetch suggestions (free, no key), one request per seed, 1 s apart:
   `curl -s "https://suggestqueries.google.com/complete/search?client=firefox&q=$(python3 -c 'import urllib.parse,sys;print(urllib.parse.quote(sys.argv[1]))' "SEED")"`
   Save raw output to `discovery/idea-scan-SNN-autocomplete.txt`. Count how many distinct brands or models appear per seed.
3. For the top 5-6 seeds, run one standard WebSearch each for existing tools. Note what each does badly (averages instead of exact models, one brand per page, spam PDFs on page 1). Do not open pages yet.
4. Find independent evidence with `evidence hn|se|gh` (product-research skill) for the top 3.
5. Rank in a table: demand (autocomplete breadth), competitor gap, data effort, harm if wrong. Write to `discovery/idea-scan-SNN.md`, and list probed-and-dropped ideas so they aren't repeated. Add the top ones to IDEAS.md.
6. Check data access before ranking high: can you read the data from public pages (maker sitemaps, sometimes only in the playwright browser)? Purina-style 403s are a "no".
7. Hand the winner to the product-manager (product-research skill).

## Lessons
- Saturated: passport validity, final paycheck by state, seed starting dates, ski DIN, battery equivalents.
- Coverage is the product in lookup tools; scope the data to what you can actually collect.
