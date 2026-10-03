# Edge Log Analyzer

A single-file, client-side dashboard for analyzing CDN/edge/WAF access logs — no server, no build step, no data leaves your browser.

Open `index.html` directly, or drag & drop (or choose) your raw log file(s) to parse them in place.

## What it shows

- KPI cards: total requests, 404 rate, 5xx count, p95 latency, peak RPS, WAF/challenge blocks
- Requests-per-second and p95-latency timelines
- Response code distribution (2xx/3xx/4xx/5xx)
- Top failing/blocked endpoints and top slowest endpoints
- A sortable, filterable table of the most active/suspicious client IPs
- Lightweight security-pattern detection (path traversal, SQLi/XSS probes, config-file scanning)

Loads with a synthetic sample dataset by default so you can see it working before uploading anything.

## Supported log format

Parses an Akamai-style edge/WAF delivery log: space-delimited records with a
record-type marker (`r` = client-facing request, `f` = origin/forward leg) at
field index 1. If your file doesn't match this shape, the tool will tell you
rather than silently showing garbage.

## Tech

Vanilla HTML/CSS/JS + [Chart.js](https://www.chartjs.org/) via CDN. Everything —
parsing, aggregation, charting — runs in the browser.
