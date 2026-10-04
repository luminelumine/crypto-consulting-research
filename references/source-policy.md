# Source and Evidence Policy

## Source hierarchy

Prefer sources in this order when they address the same claim:

1. On-chain records, official filings, court or regulator documents, and reproducible API data.
2. Protocol documentation, governance proposals, audits, source repositories, and direct company disclosures.
3. Established independent data providers and research organizations with disclosed methodology.
4. Reputable reporting with named sources.
5. Public social posts, community discussions, and anonymous claims.

Lower-ranked sources can reveal hypotheses and sentiment but should not overrule stronger evidence without explanation.

## Evidence ledger

For substantial reports, keep a compact ledger with:

| Field | Meaning |
|---|---|
| Claim ID | Stable identifier used in notes |
| Claim | Exact proposition being supported |
| Source URL | Direct page, API endpoint, document, post, or transaction |
| Published / observed / retrieved | Publication date, actual data observation time, and retrieval time with timezone |
| Source type | Primary, independent data, reporting, or social |
| Identity and scope | Chain, contract or project ID, venue, requested and covered period |
| Units and definitions | Currency, denominator, aggregation, metric definition, and provider methodology |
| Reproduction | Credential-free CLI command, query, inputs, and relevant response metadata |
| Supports / contradicts | Relationship to the claim |
| Limits | Missing coverage, methodology concerns, or conflicts |

## Citation rules

- Link to the exact supporting page, not a homepage or search-results page.
- Do not use a citation for a claim it does not directly support.
- Attribute estimates and forecasts to their author and date.
- Keep short quotations within copyright limits; prefer paraphrase.
- Label calculations as analyst calculations and cite every material input.
- When sources conflict, present the disagreement and explain which definition or source is used.
- Surf is a retrieval layer, not automatically the origin of every claim. Preserve the underlying provider or document when available and do not treat two wrappers of the same dataset as independent evidence.
- If a CLI result has no accessible public record, identify Surf and the endpoint or operation, inputs, data time, and limitations in a source note. Do not fabricate a public permalink or cite a documentation page as proof of the returned value. Primary documents are still needed for claims about product behavior and official announcements.
- Never publish authentication headers, credential files, private portfolio data, or unrelated conversation content as evidence. Sanitize retained responses and use only the public addresses needed for the authorized analysis.

## Social evidence

Social content is useful for discovering objections, narratives, user language, and emerging issues. It is not a representative survey unless the sampling design supports that claim. Record the platform, search terms, window, sample size, inclusion rules, and visible engagement. Note bot, promotional, survivorship, regional, and language biases where relevant.
