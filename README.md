# Deadline Planner

Buildless, private-review browser app for S028. Serve `dist/` directly. Routes: Korean `/`, English `/en/`, Spanish `/es/`, Simplified Chinese `/zh/`, Japanese `/ja/`, German `/de/`. Every route has a manual language menu and complete static UI, error, example and help catalogs. No remote dependencies, upload, tracking or automatic persistence.

## Behavior and assumptions

1. Enter the first planning date and one shared inclusive deadline.
2. Break work into up to 30 independent, divisible tasks. Give each a name, estimated hours and a priority from 1 (highest) to 5.
3. Enter available hours explicitly for each date, including weekends and holidays: one `YYYY-MM-DD,hours` line per date. Omitted dates have zero capacity. Duplicate dates, dates outside the planning window and capacities outside 0–24 hours are rejected.
4. Add an integer buffer from 0 to 100 percent. Estimated hours round upward to minutes; daily capacity rounds downward; buffer rounds upward per task.
5. Capacity is reserved in priority order, with input-order ties. Each task consumes the last available dates first, working backward from the shared deadline. Higher priority reserves capacity before lower priority; it does not imply earlier calendar placement.
6. Review chronological daily allocations, unused daily capacity and each task's planned/missing minutes. Insufficient capacity is reported explicitly and the partially allocated plan remains exportable. This is a planning assumption, not a completion guarantee.

Tasks do not have dependency constraints, individual deadlines or time-of-day blocks. Work can be split across days. The date window is at most 366 calendar dates in years 1900–2100; dates use strict calendar arithmetic without timezone conversion. Hour inputs allow up to two decimal places and task estimates are limited to 1000 hours. The capacity text input is limited to 20,000 characters.

CSV export/copy is UTF-8 and includes scheduled, idle and unplanned records. Task names with formula-start characters receive a leading apostrophe to reduce spreadsheet formula execution. Quoting preserves commas, quotes and newlines. Daily capacity and remaining capacity repeat per assignment row; those two columns must not be summed. Both behaviors are documented in all six languages.

## Verification

Executed from this directory through the explicit workspace RTK `proxy` wrapper and bundled Node runtime:

```text
node --check dist/core.js
node --check dist/app.js
node test.mjs
```

All exited 0. Fixtures passed exact 180-minute backward placement across two dates, 20% buffer yielding 144 required minutes, priority reservation with an explicit 60-minute shortfall, conservative 0.01-hour rounding, zero availability, strict leap-date/window checks, duplicate/out-of-window/overfull daily capacity, invalid buffer/tasks/names, quoted multiline formula-safe CSV and complete catalogs/routes in six languages. Browser visual QA is not claimed.

Date format reference: https://html.spec.whatwg.org/multipage/common-microsyntaxes.html#dates . All code, examples and interface assets are original. No third-party assets are included.

Release route regression: `node test.mjs` rerun after repairing static route generation. All six HTML titles now match their locale catalogs. Every page has the correct document language and exactly one menu identity each for ko/en/es/zh/ja/de. All existing functional fixtures and the new static-title/menu regression exited 0.

## Public GitHub Pages release

Launch: https://nanalab.kr/deadline-planner/

Korean is at the root. English, Spanish, Chinese, Japanese and German use `/en/`, `/es/`, `/zh/`, `/ja/`, `/de/`.
GitHub Pages serves `main` → `/docs` using the branch build. No project CNAME is used.

Build: `node scripts/build-public.mjs`. The `dist/` snapshot and its original generators/tests are preserved as the application source. The public build adds metadata, six-locale sitemaps, original JPEG cards, More Apps styling and the existing NaNa publisher integrations. It does not upload selected user files.

Publisher integrations: AdSense, Google Tag Manager, Google Analytics and Cloudflare Web Analytics, with the same publisher settings as the NaNa Lab homepage. These scripts can make network requests independently of local file processing.

Original JPEG cards are rendered from original text and geometry using an installed system font; no font files are redistributed. Existing source references and third-party notices remain applicable.
