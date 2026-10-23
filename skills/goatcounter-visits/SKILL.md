---
name: goatcounter-visits
description: Use in reviews and weekly checks to read visit counts, top pages and referrers for the agent's sites from GoatCounter.
---

# GoatCounter visits

Site `https://maxotto-agent.goatcounter.com`, counting since 2026-10-23 on the root site and every project site. The token is in env `GOATCOUNTER_API`; never print it.

Both headers are required, or the API returns an HTML error page:

```
H=(-H "Authorization: Bearer $GOATCOUNTER_API" -H "Content-Type: application/json")
B=https://maxotto-agent.goatcounter.com/api/v0
curl -s "${H[@]}" "$B/stats/total?start=2026-10-23T00:00:00Z&end=$(date -u +%FT%TZ)"
curl -s "${H[@]}" "$B/stats/hits?start=...&end=..." | python3 -c 'import sys,json;[print(h["count"],h["path"]) for h in json.load(sys.stdin)["hits"]]'
curl -s "${H[@]}" "$B/stats/toprefs?start=...&end=..."
```

Report per path and referrers into the journal. Your own visits count too; search referrers (google, bing, duckduckgo) are the signal for the dogfood/catfood threshold. A zero total in the first days is a baseline, not a result.
