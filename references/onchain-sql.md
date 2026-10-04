# Custom on-chain analysis

Use this reference for SQL and custom aggregates. The live catalog, schema, endpoint help, and coverage records are authoritative. This skill does not bundle a frozen table catalog.

## Discover before querying

```bash
surf catalog search "dex trades"
surf catalog show ethereum_dex_trades
surf catalog practices
surf onchain-sql --help
```

Adapt the search and table to the question. Inspect columns, units, address conventions, partition keys, and coverage. If catalog access is unavailable, use current schema discovery rather than guessing a table or schema.

## Query constraints

- Submit a JSON object containing `sql`, not bare SQL; inspect current CLI help for the supported input mechanism. For stdin-capable versions, a prepared `query.json` can be passed as `surf onchain-sql --json < query.json`.
- Use read-only `SELECT` or `WITH` and fully qualified `agent.TABLE` names. Select needed columns and bounded output rather than scanning everything.
- Large fact tables require a meaningful `block_date` lower bound and a window of at most 365 days. Each large table in a join, subquery, or union branch needs its own bound. For longer supported history, query smaller windows and reconcile boundaries.
- Use the catalog's exact partition and query practices if they impose additional or updated constraints. A row limit bounds output, not scan cost; aggregate server-side where suitable.
- Resolve token identity outside SQL, then filter by the actual contract. Do not search unindexed symbol columns or wrap address columns in `lower()`; normalize EVM address literals according to the schema without changing case-sensitive non-EVM identifiers.
- For transfers involving an address as sender or recipient, follow the catalog's separate-branch approach rather than an expensive `OR`. Account for self-transfers before adding counts or amounts.
- Distinguish decimal-adjusted `amount` from base-unit `amount_raw`. Inspect price coverage before converting units to USD. Missing prices are missing values, not zero value.

## Coverage before interpretation

Record earliest supported history, latest complete date or block, internal gaps, included protocols, and event semantics. Retrieval now does not make a dataset real-time. Compare periods with matching completeness; label partial days.

For Robinhood Chain, discover its dataset coverage view before interpreting historical bounds. Its public launch date does not necessarily mark the start of canonical mainnet history; apply a launch-period cutoff only when requested. Mapped swaps need not cover every pool, event transfers may exclude native or internal transfers, supported-token prices are not universal price coverage, and current token metadata is not a historical snapshot.

## Timeouts and asynchronous jobs

The synchronous query path has execution and row limits documented by current help. A timeout is not a reason to automatically resubmit. Prefer a narrower query; use the available asynchronous interface only when the scope requires it and the user's access and cost constraints permit it. Follow its idempotency guidance and polling interval, then retrieve all result pages. Do not create duplicate jobs or imply completion while the job is queued or running.

Save the credential-free query, requested window, observed coverage, sanitized result, and material calculations when they are needed to reproduce a report conclusion.
