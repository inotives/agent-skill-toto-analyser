# Singapore Toto CLI

```sh
npm install
npm run build

# Show all commands and estimator methods.
npm run toto -- help

See [Skill.md](Skill.md) for the complete operating workflow, including
refreshing the latest result before running estimators.

If the database is missing, CLI initialization creates an empty database and
will warn that you must import data or run `refresh` afterward.

# One-time CSV import (the default path is data/ToTo.csv)
npm run import

# Download historical Toto results from:
# https://en.lottolyzer.com/history/singapore/toto/page/1/per-page/50/summary-view
# Save the downloaded CSV anywhere, then point --path to that file.
npm run toto -- import --path /path/to/other.csv

# Explicitly crawl Lottolyzer and upsert all draws found on its page.
npm run toto -- refresh

# Show the latest local result; after the expected Monday/Thursday 7pm SGT
# cutoff, refresh from Lottolyzer when the local database is behind.
npm run latest

# Frequency-based estimate from the latest 624 draws (approximately six years).
npm run predict

# Choose another estimator:
npm run toto -- predict --estimator_method recency
# Run several estimators in one command:
npm run toto -- predict --estimator_method recency,overdue,ensemble

Available estimator methods:

- `frequency` — most common numbers.
- `recency` — weights newer draws more heavily.
- `bayesian` — smoothed frequency estimates.
- `overdue` — favors numbers absent for more draws.
- `hot-cold` — combines frequency and time since appearance.
- `pairs` — uses historical number co-occurrence.
- `balanced` — applies number-range and odd/even balance constraints.
- `sum-range` — favors tickets near a typical number sum.
- `monte-carlo` — samples 10,000 candidate tickets.
- `ensemble` — combines frequency, recency, overdue, and pair scores.
```

The SQLite database is `data/singapore_toto.sqlite3`. `latest` also accepts the
alias `check`; use `npm run toto -- init` to create the table without importing.
Predictions are descriptive frequency estimates and cannot improve random-lottery odds.
