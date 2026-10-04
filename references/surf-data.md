# Surf data workflow

This integration uses Surf as a data-access tool. Consult the [official CLI guide](https://agents.asksurf.ai/docs/cli/cli) for installation and authentication, and the [upstream skill repository](https://github.com/asksurf-ai/surf-skills) for changes. Current CLI help and the synced API specification override examples in this reference.

## Start a data session

1. Check that `surf` is available and inspect its version. If missing, explain the dependency and follow the official installation guide within the user's authorization. Do not silently install a second skill or execute a downloaded installer.
2. Run `surf sync` once before discovery in the session. If synchronization fails, disclose use of a cached specification and do not imply that command coverage is current.
3. Run `surf list-operations`; filter the output for the subject. Inspect `surf COMMAND --help` before constructing every selected endpoint's request. Use exact flag names, enum values, and required inputs from that help.
4. Execute the narrowest useful query with `--json`, keeping `data`, `meta`, and any `error`. Some responses are objects and others arrays; inspect the envelope before parsing.

Do not run `npx surf`; it need not refer to the installed Surf CLI. Installing or updating the CLI is a separate setup operation, not a requirement for each research task.

## Discovery map

These are starting keywords and candidate commands, not a guaranteed catalog. Search the current operation list before deciding a capability is absent.

| Research need | Search keywords / candidate commands |
|---|---|
| Price, capitalization, supply, ranking, derivatives | `market`, `exchange` |
| Funding, order books, venue candles | `exchange`, `hyperliquid` |
| DEX candles for a token contract | `dex-token-price` |
| Project identity, TVL, fees and revenue | `project-detail`, `project-defi-metrics`, `project-defi-ranking` |
| Holder concentration, transfers, unlocks | `token-holders`, `token-transfers`, `token-tokenomics` |
| Exact ticker to contract candidates | `search-token` |
| Public wallet holdings, history, DeFi positions | `wallet` |
| Trader positions and fill history | `hyperliquid` |
| Fund profile and portfolio | `fund-detail`, `fund-portfolio`, `search-fund` |
| Funding rounds, investors, token sales | `search-fundraising` |
| Events, news, primary-source discovery | `search-news`, `search-events`, `news-detail`, `search-web`, `web-fetch` |
| Prediction markets, liquidity and odds | `polymarket`, `kalshi`, `prediction-market`, `matching` |
| Chain transactions and custom aggregates | `onchain`, `catalog`; read [onchain-sql.md](onchain-sql.md) |
| Public X posts by known IDs | `x-tweets`; verify current social coverage before assuming a search endpoint |

Translate non-English search intent into suitable entity names or English keywords where the API needs them, then answer in the user's language.

## Resolve identity before measuring

- Inspect the relevant detail endpoint first. A name-based `project-detail` or `fund-detail` may already accept `--q`; an endpoint that requires an ID needs search or a provided identifier first.
- `search-token --q` resolves an exact ticker, case-insensitively. It does not accept a fuzzy name, contract, or trading pair. Fuzzy project names belong in project discovery; exchange pairs belong in exchange endpoints.
- Preserve the returned `(chain, address)` pair and confirm it against the project's official contract listing or explorer when identity affects the conclusion. Multiple assets can share a ticker; state a defensible selection or ask when ambiguity remains material.
- `search-token`'s `volume_usd` is a compatibility placeholder that returns zero. It is not measured volume and cannot rank candidates or prove inactivity.
- A user-provided contract can go directly to a compatible token or DEX endpoint. Check chain support rather than guessing or switching chains. Use canonical enum names from help, such as `ethereum`, `solana`, `arbitrum`, and `bsc`.

## Endpoint distinctions that affect conclusions

- **DEX candles:** `dex-token-price` takes chain and address; `token-dex-trades` returns swaps. A market-price series is not a replacement for contract-specific DEX coverage. Inspect interval and range limits, and both boundaries for custom windows.
- **Fundraising versus funds:** use fundraising search for rounds and timelines, fund endpoints for investor profiles and portfolios. Confirm announcements against original disclosures.
- **Events versus project feeds:** a project-tagged feed can miss an event whose articles lack that tag. Search event keywords before interpreting an empty feed as no news.
- **Prediction markets:** record the contract's question, resolution criteria, venue, quote time, liquidity, and spread. Market prices are market-implied signals, not independently verified probabilities or forecasts.
- **On-chain indicators:** inspect asset coverage as well as flags. Some metrics cover BTC only; empty coverage is not a zero metric.
- **TVL and revenue:** check chain, protocol grouping, denomination, time window, and the provider's definitions. TVL alone does not establish active users, profitability, or value flowing to token holders.

Flags are typically kebab-case; use `--q` for searches, never `-q`. Do not transplant flags between endpoints or invent aliases.

## Completeness and pagination

Follow documented cursors or offsets until the requested scope is covered. Keep filters stable. A short or empty page may still have a continuation cursor; inspect `meta` rather than stopping on row count alone. Detect repeated cursors or lack of progress and stop with an explicit completeness limitation.

If a time-series endpoint reports truncation but has no cursor or offset, split supported time windows and de-duplicate overlapping boundaries. Inspect timestamp units, timezone, whether end boundaries are inclusive, and whether the latest bucket is complete. Report the covered interval and any remaining gaps.

For complete **Hyperliquid fill history**, inspect `hyperliquid-fills --help`, begin in ascending order with an explicit start, and follow `meta.next_cursor`. Subsequent cursor requests may allow only the cursor, symbol, and limit; use the current help contract. The default descending mode is a capped recent view and must not be summed as lifetime PnL. De-duplicate by fill ID, not transaction hash or seconds. Fill-level PnL omits funding; account returns also require transfers and unrealized value. Choose closed-trade or account endpoints when the question requires those semantics.

If the requested period, ranking, or granularity is unsupported, label the closest available proxy or fetch a more suitable source. Never present total-TVL rankings as TVL-growth rankings or an overall leaderboard as a requested seven-day ranking.

## Authentication, quota, and failures

Try the intended query using existing authentication; do not request keys before a failure. Avoid debug logs that might contain request details. On failure, inspect the returned error internally and explain the practical limitation in plain language.

| Condition | Response |
|---|---|
| Invalid or missing authentication | Direct the user to official authentication setup in their own terminal; never handle a pasted key. Use supported anonymous access only if available without changing their stored credentials. |
| Anonymous quota or paid balance exhausted | Explain that Surf access is exhausted. Continue with available authorized public sources and disclose the gap; do not purchase credits. |
| Rate limit or temporary transport problem | Back off briefly and retry once. Stop repeated attempts and use an appropriate fallback if the failure persists. |
| Unknown command, flag, or enum | Refresh discovery if needed and inspect help; do not cycle through guessed parameters. |
| Empty data | Verify identity, chain, period, coverage and response metadata. Missing observations are not zero activity. |
| SQL timeout | Follow the structured recommended action and [onchain-sql.md](onchain-sql.md); do not automatically repeat the same synchronous query. |

Quota and pricing can change; do not promise a fixed free allowance or query cost. Do not send feedback or conversation content automatically.
