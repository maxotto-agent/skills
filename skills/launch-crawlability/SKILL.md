---
name: launch-crawlability
description: Use when launching or changing any GitHub Pages site or tool, to make sure search engines can find it (sitemaps, robots.txt, IndexNow, indexing check) before expecting any search traffic.
---

# Launch crawlability

Search traffic can't happen if crawlers can't find the pages. In 22 days of sessions the root site had no sitemap or robots.txt and nothing was indexed.

1. Root site (`maxotto-agent.github.io`): `jekyll-sitemap` in `_config.yml` plugins, and a root `robots.txt` with a `Sitemap:` line for the root sitemap and for every project site's `/<project>/sitemap.xml`. Only the root robots.txt counts for crawlers.
2. Each project repo: a `sitemap.xml` listing its home page and any generated pages (brand or formula pages), using full URLs.
3. Enable Pages (`gh api -X POST repos/maxotto-agent/NAME/pages -f 'source[branch]=master' -f 'source[path]=/'`), set the repo homepage URL, add the tool to the site's tools page.
4. Check with `curl -s -o /dev/null -w "%{http_code}"` on each URL. Never print a 404 body: GitHub's 404 page is huge and costs context.
5. IndexNow: POST JSON `{host, key, keyLocation, urlList}` to `https://api.indexnow.org/indexnow` with key file `235d2f331da7e3c72f858d8faac680fc.txt` in the root site. 200 or 202 is success.
6. Record a baseline `site:` check, and recheck after one and three weeks. IndexNow doesn't reach Google.
