# Diagnostics and performance

Use this reference for logs, metrics, APM, caching symptoms, worker pressure, HTTP status interpretation, and performance attribution.

## Evidence sources

### NGINX/web logs

NGINX logs cover requests that reached origin. Requests served entirely from Edge Cache are absent, so low origin-log volume does not establish low total traffic.

Correlate exact timestamp, timezone, URL, method, status, source IP, user agent, and origin response time. Validate the actual earliest and latest timestamps inside exports rather than trusting filenames or UI labels.

Source: [Accessing Web Server Logs](https://pressable.com/knowledgebase/accessing-web-server-logs-for-your-site/).

### PHP logs

PHP logs can confirm recorded application warnings and fatals, but no matching entry does not prove normal completion. A memory-exhaustion fatal shows where the final allocation failed, not necessarily what initiated or consumed the workload.

Source: [Accessing WordPress Error Logs](https://pressable.com/knowledgebase/accessing-wordpress-error-logs/).

### Activity logs

Activity logs record account and site actions, not every runtime request or event. Use them to correlate configuration, access, domain, backup, collaborator, and dashboard activity.

Source: [Pressable Activity Logs](https://pressable.com/knowledgebase/pressable-activity-logs/).

### Response headers

Cache state and a coarse timing measurement are visible in response headers on any request, with no credentials and no dashboard access. Capture with `curl -sI https://DOMAIN/PATH` or the browser network tab.

- `x-ac: 3.sea _atomic_bur HIT` carries the edge cache state. `HIT`, `MISS`, and `BYPASS` are the common values. `BYPASS` normally means the response was deliberately not cacheable: a logged-in request, a cart or checkout route, a REST endpoint.
- `server-timing: a8c-cdn, dc;desc=sea, cache;desc=HIT;dur=884.0` repeats the cache state and adds `dur`, the duration the edge recorded for that response. On a `HIT` that reflects cache service time; on a `MISS` or `BYPASS` it is dominated by the origin fetch.

`dur` on an uncached request is the cheapest first measurement in a slow-page investigation. A slow page with a low `dur` is spending its time somewhere other than origin PHP: the browser, the network, render-blocking assets, or third-party requests. A high `dur` justifies an APM capture. A single request is a single sample, so repeat it before concluding anything, and note that these headers reflect the edge's view rather than a breakdown of work inside PHP.

## APM Insights workflow

APM is a temporary, deliberately activated capture—not continuous proof about every request.

1. Choose a short capture window.
2. Activate APM with the user's involvement when required.
3. Reproduce the exact slow or failing PHP operation during that window.
4. Record route and timestamp.
5. Compare total duration with WordPress, database, Memcache, and external-call spans.
6. Preserve relevant trace details or the temporary share link promptly.

No relevant trace is inconclusive if the intermittent operation was not captured. Explain temporary diagnostic overhead and avoid broad, unfocused capture periods.

Source: [Using APM Insights](https://pressable.com/knowledgebase/using-apm-insights-to-troubleshoot-performance-issues/).

## PHP workers and concurrency

A PHP worker processes one PHP request at a time. More workers increase concurrency; they do not make one intrinsically slow request faster.

- Verify current plan allowance rather than hardcoding worker, CPU, memory, or burst values.
- Confirm saturation, queueing, or repeated capacity exhaustion before recommending more capacity.
- Inspect slow PHP, SQL, external calls, lock contention, and memory behavior independently.
- Remember that logged-in traffic, uncached pages, REST, WP-Cron, managed cron, imports, exports, backups, webhooks, and external calls may occupy workers.
- Edge hits do not use workers. Object-cache hits still do.

Sources: [PHP Workers and Concurrency](https://pressable.com/knowledgebase/php-workers-concurrency-and-what-they-mean-for-your-site/), [Diagnosing PHP Worker Saturation](https://pressable.com/knowledgebase/diagnosing-and-mitigating-php-worker-saturation/).

## HTTP status and edge reasoning

Do not merge browser-visible `403`, `406`, `429`, and origin `599` events into one diagnosis.

- A documented Pressable WAF block uses `406`; a `403` requires investigation of application plugins, external proxies/services, crawler verification, and edge challenges as well as possible platform involvement.
- A logged `429` can reflect rate controls. A browser-visible `429` may correlate with a resource-related `599` in origin evidence.
- Automatic Defensive Mode can be a hypothesis during sudden resource pressure even if the user did not enable it manually. Its JavaScript challenge can block crawlers, APIs, webhooks, previews, and monitors that cannot execute it.
- Edge-handled requests may be absent from origin logs.
- Never infer automatic Defensive Mode from `403` alone or expose, guess, or promise thresholds, durations, decay behavior, exemptions, allowlists, or immediate release. Never use private support cases or another user’s incident to fill gaps in public documentation.

Collect the exact status, body, headers, cookies, URL, method, user agent, source IP, timestamp, and matching origin-log presence. Contact Support when effective edge state requires internal visibility.

Sources: [Understanding 429 Errors](https://pressable.com/knowledgebase/understanding-429-errors-and-edge-cache/), [Why Is My Site Displaying a 406 Error?](https://pressable.com/knowledgebase/why-is-my-site-displaying-a-406-error/).

## Performance reasoning guardrails

Do not treat any of these as causation without correlation and controlled testing:

- plugin installation or a stack-frame appearance;
- one slow request;
- one cache miss;
- database size or autoload totals;
- cron timing;
- high query count without duration/context;
- low origin-log volume;
- the filename where memory exhaustion terminated.

Prefer a request-scoped model: reproduce one operation, correlate timestamps across evidence, identify the dominant time/resource span, test a falsifiable explanation, and compare before/after behavior under similar conditions.
