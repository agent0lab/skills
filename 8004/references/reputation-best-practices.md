# Reputation Best Practices

Recommended patterns for submitting and consuming reputation feedback.

Source: [erc-8004/best-practices Reputation.md](https://github.com/erc-8004/best-practices/blob/main/Reputation.md)

## tag1 Suggested Values

Standardized `tag1` values enable aggregators and consumers to compare agents consistently:

| tag1 | What it measures | Example human value | `value` | `valueDecimals` |
|------|------------------|---------------------|---------|-----------------|
| `starred` | Quality rating (0-100) | `87/100` | `87` | `0` |
| `reachable` | Endpoint reachable (binary) | `true` | `1` | `0` |
| `ownerVerified` | Endpoint owned by agent owner (binary) | `true` | `1` | `0` |
| `uptime` | Endpoint uptime (%) | `99.77%` | `9977` | `2` |
| `successRate` | Endpoint success rate (%) | `89%` | `89` | `0` |
| `responseTime` | Response time (ms) | `560ms` | `560` | `0` |
| `blocktimeFreshness` | Avg block delay (blocks) | `4 blocks` | `4` | `0` |
| `revenues` | Cumulative revenues (e.g., USD) | `$560` | `560` | `0` |
| `tradingYield` (`tag2` = `day, week, month, year`) | Trading Agent Yield | `-3.2%` | `32` | `1` |

> ⚠ Note: The source shows `tradingYield` with a human value of `-3.2%` but a `value` of `32` (not `-32`). The sign is not encoded in the on-chain value field per the source table.

### Star Rating Mapping

Map 1-5 star ratings to 0-100 scale with `valueDecimals = 0`:

- 1★ → value = 20
- 2★ → value = 40
- 3★ → value = 60
- 4★ → value = 80
- 5★ → value = 100

## Use Cases

### 1. Curation in Catalogs

Catalogs let users rate agents with 1-5 stars stored as `tag1 = starred` on the 0-100 scale. Once published, curation from one catalog informs ordering on every other catalog. Catalogs should let users filter by who rated (`clientAddress`). When the rater is an agent, it SHOULD submit from its `agentWallet` so UIs can resolve the rater's identity.

### 2. Agent Reliability

Trusted parties (infra companies, watchtowers, data providers) routinely probe agents and publish periodic signals. Common dimensions:

- Reachability — `tag1 = reachable` (binary)
- Uptime — `tag1 = uptime` (percentage)
- Success rate — `tag1 = successRate` (percentage)
- Latency — `tag1 = responseTime` (ms)

Because signals are public, consumers choose their own trusted sources by filtering by `clientAddress`.

### 3. Trading Agent Performance

Independent rankers track on-chain performance and publish signals using `tag1 = tradingYield` and `tag2` for the time window (`day`, `week`, `month`, `year`). Consumers choose which rankers to trust via `clientAddress`.

### 4. Revenue Performance

Cumulative revenue is a signal for clients to assess if an agent is battle-tested. Revenue is best known by the facilitator. ERC-8004 lets those parties publish standardized revenue signals (`tag1 = revenues`).

## Expandability with feedbackURI

For simplicity, the use cases above focus on on-chain fields only. ERC-8004 is designed to be expandable: feedback can optionally include an off-chain file URI (e.g., IPFS URI) with richer context, plus a `feedbackHash` for integrity.

```
feedbackURI: ipfs://Qmxxx.../feedback.json
feedbackHash: 0xabcd...1234
```

`feedbackHash` is the keccak256 of the `feedbackURI` content (optional for IPFS, where content-addressing provides integrity).

## Submission Rules

- Submitter MUST NOT be the agent owner or approved operator
- `valueDecimals` MUST be 0-18
- `tag1`, `tag2`, `endpoint`, `feedbackURI`, `feedbackHash` are all OPTIONAL
- When feedback is given by an agent, it SHOULD use its `agentWallet` address as `clientAddress`

## Example Feedback

```solidity
// Quality rating (4★ = 80)
giveFeedback(agentId, 80, 0, "starred", "", "", "", bytes32(0))

// Uptime measurement
giveFeedback(agentId, 9977, 2, "uptime", "", "", "", bytes32(0))

// Trading performance (weekly yield)
giveFeedback(agentId, 32, 1, "tradingYield", "week", "", "", bytes32(0))

// Revenue signal with off-chain context
giveFeedback(agentId, 560, 0, "revenues", "", "", "ipfs://Qm...", keccak256(...))
```
