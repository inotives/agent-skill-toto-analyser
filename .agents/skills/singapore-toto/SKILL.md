---
name: singapore-toto
description: Use this skill whenever the user asks about Singapore Toto results, the latest Toto draw, refreshing or importing Toto history, checking whether a draw is available, or estimating numbers from historical Singapore Toto data. Run the repository CLI from the project root and clearly distinguish local results, refreshed results, and statistical heuristics.
---

# Singapore Toto analyser

Use the repository CLI as the source of truth. Run commands from:

```text
/Users/inotives/workspaces/agent-skill-toto-analyser
```

## Result requests

For “latest result”, “check Singapore Toto”, or similar requests, run:

```sh
npm run latest
```

The CLI checks the local SQLite database and refreshes from Lottolyzer when
the expected Monday/Thursday draw is missing after 7pm Singapore time. Report
the draw number, date, six winning numbers, and additional number. If refresh
fails, say that the answer is the latest local result and include the refresh
failure rather than presenting stale data as current.

## Import and refresh

Use these commands when explicitly requested:

```sh
npm run import                         # only when data/ToTo.csv exists
npm run toto -- import --path /path/to/ToTo.csv
npm run toto -- refresh
```

Use `npm run toto -- init` only to create the database/table without importing
rows. The database is `data/singapore_toto.sqlite3`. The default CSV path is
`data/ToTo.csv`; if it is absent, use an explicit `--path` instead.

## Estimates

For “predict”, “estimate”, or “suggest numbers”, run the requested estimator:

```sh
npm run toto -- predict --estimator_method ensemble
```

Supported methods include `frequency`, `recency`, `bayesian`, `overdue`,
`hot-cold`, `pairs`, `balanced`, `sum-range`, `monte-carlo`, and `ensemble`.
Several methods can be compared with a comma-separated value, for example:

```sh
npm run toto -- predict --estimator_method frequency,recency,ensemble
```

Always label the output as a statistical heuristic. Singapore Toto draws are
random; these estimates cannot improve the mathematical odds of a ticket.

## Maintenance

If the CLI source or dependencies changed, run this before using it:

```sh
npm install
npm run build
```

Do not manually edit the SQLite database when the CLI has an import or refresh
command for the requested operation.
