---
name: singapore-toto
description: Use this skill when the user asks to check Singapore Toto results, refresh local Toto history, import Toto CSV data, or estimate numbers using historical Singapore Toto draws.
---

# Singapore Toto Skill

Use the TypeScript CLI in this repository to maintain local Singapore Toto
results and generate heuristic number estimates.

## Setup

Run once after cloning or changing dependencies:

```sh
npm install
npm run build
```

The SQLite database is stored at:

```text
data/singapore_toto.sqlite3
```

The default historical CSV is:

```text
data/ToTo.csv
```

If a command other than `init` is run before the database exists, the CLI warns
that initialization creates an empty database. Answer `y` to continue, then
import the CSV or run `refresh`; answer `n` to cancel safely.

## Import historical CSV data

Import the default CSV with:

```sh
npm run import
```

For a CSV saved elsewhere, provide its path:

```sh
npm run toto -- import --path /path/to/ToTo.csv
```

Historical Toto data can be downloaded from:

```text
https://en.lottolyzer.com/history/singapore/toto/page/1/per-page/50/summary-view
```

The import creates `toto_results` if it does not exist and upserts existing
draws safely.

## Update the latest result before estimating

Always refresh the local database before running an estimator:

```sh
npm run toto -- refresh
```

`refresh` crawls the Lottolyzer history page, stores the available draws, and
prints the latest draw and date. Then run the estimator:

```sh
npm run toto -- predict --estimator_method ensemble
```

The normal result command also checks freshness automatically:

```sh
npm run latest
```

Results are expected after 7pm Singapore time on Mondays and Thursdays. If
the expected draw is missing, `latest` attempts the same online refresh.

If the online site is unavailable, use the most recent local result and retry
the refresh later. Do not present stale data as the latest draw without saying
that it came from the local database.

## Estimator methods

The estimator window is 624 draws, approximately six years:

- `frequency`: most common numbers.
- `recency`: weights newer draws more heavily.
- `bayesian`: applies frequency smoothing.
- `overdue`: favors numbers absent for more draws.
- `hot-cold`: combines frequency and time since appearance.
- `pairs`: uses historical number co-occurrence.
- `balanced`: applies range and odd/even constraints.
- `sum-range`: favors tickets near a typical total-number sum.
- `monte-carlo`: samples 10,000 candidate tickets.
- `ensemble`: combines frequency, recency, overdue, and pair scores.

Run one method:

```sh
npm run toto -- predict --estimator_method recency
```

Run several methods:

```sh
npm run toto -- predict --estimator_method frequency,recency,ensemble
```

Predictions are statistical selection heuristics. Singapore Toto draws are
random, so these methods cannot improve the mathematical odds of a ticket.

## Useful commands

```sh
npm run toto -- help
npm run toto -- init
npm run toto -- import
npm run toto -- latest
npm run toto -- check
npm run toto -- refresh
npm run toto -- predict --estimator_method ensemble
```
