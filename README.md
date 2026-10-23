# Edge Log Analyzer

> **Generated — don't edit `index.html` or `log-line-fields.pdf` here.** They're built from
> [`public/tools/edge-log-analyzer/`](https://github.com/abilashhp/iamabhilash-site/tree/main/public/tools/edge-log-analyzer)
> in [iamabhilash-site](https://github.com/abilashhp/iamabhilash-site) by `scripts/build-standalone-ela.mjs`
> and committed here. Direct edits will be overwritten.

A single-file, client-side dashboard for analyzing CDN/edge/WAF access logs — no server, no build step, no data leaves your browser.

Open `index.html` directly, or drag & drop (choose, or paste) your raw log file(s) to parse them in place.

## What it shows

- **Time Range** control: zoom into a window and every widget below — including Custom
  Query — re-analyzes just that slice. Useful for isolating a burst/incident inside a much
  longer log file instead of averaging it away across the full observed window.
- KPI cards: total requests, 404 rate, 5xx count, cache hit ratio, total bytes served,
  WAF info coverage, p95 latency, peak RPS, WAF/challenge blocks
- Requests-per-second, p95-latency, bandwidth, and status-codes-over-time timelines (the
  last one plots 2xx/3xx/4xx/5xx as separate lines so a spike in one class stands out)
- Response code distribution (2xx/3xx/4xx/5xx)
- Cache status breakdown (decoded per-letter from the real Ghost Object Status flags, with
  hover tooltips explaining each flag) and Content-Type distribution
- Top failing/blocked endpoints, top slowest endpoints, top WAF rules triggered (the
  human-readable rule/attack-signature name, not just an app ID), and Client Reputation
  Signals (Akamai's own bot/threat classification, not a User-Agent guess)
- A sortable, filterable table of the most active/suspicious client IPs, including each
  IP's peak request rate in any 60-second window (velocity, not just a total count)
- Lightweight security-pattern detection (path traversal, SQLi/XSS probes, config-file
  scanning), True-Client-IP vs edge-observed Client IP mismatch detection, and log-coverage
  gap detection (flags the largest silence between requests, in case it's a logging hole
  rather than real quiet)
- **Download dashboard summary (JSON)** to export the processed KPIs/charts for a ticket
- A **Custom Query** box: describe what you want in plain English (e.g. "list ARLs of 301s",
  "top 10 client IPs", "how many requests were blocked") and it builds and runs the equivalent
  AWK + pipeline command for you — a deterministic keyword/regex translator, not an LLM, so it's
  free, instant, and never sends anything anywhere. An "Advanced" panel below it lets you write
  AWK + `sort`/`uniq -c`/`head`/`tail`/`grep`/`wc -l` pipelines directly, if you'd rather. Queries
  can be named and saved (stored in your browser's localStorage) for repeat investigations.

Loads with a synthetic sample dataset by default so you can see it working before uploading anything
(the Custom Query box requires an uploaded file, since the sample dataset doesn't ship raw log lines).

## Supported log format

Parses an Akamai-style edge/WAF delivery log: space-delimited records with a
record-type marker (`r` = client-facing request, `f` = origin/forward leg) at
field index 1. If your file doesn't match this shape, the tool will tell you
rather than silently showing garbage.

The full documented column list for both record types is in
[`log-line-fields.pdf`](./log-line-fields.pdf) (also linked from the Custom
Query panel in the app).

## Tech

Vanilla HTML/CSS/JS + [Chart.js](https://www.chartjs.org/) via CDN. Everything —
parsing, aggregation, charting — runs in the browser.
