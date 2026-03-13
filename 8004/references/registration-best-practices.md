# Registration Best Practices

Recommended patterns for creating production-ready agent registrations.

> Source: https://github.com/erc-8004/best-practices/blob/main/Registration.md

## Rule 1: Name, Image, Description

**Name**: Clear, memorable, descriptive (e.g., "DataAnalyst Pro")

**Image**: High-quality image (PNG, SVG, WebP) — displayed in wallets, explorers, and NFT marketplaces

**Description**: An elevator pitch covering:
- What the agent does (core functionality)
- How it works (brief technical approach)
- What problems it solves
- Pricing, if applicable

## Rule 2: Services Endpoints

Include all supported protocols in the `services` array:

| Service | Endpoint Format | Version | Optional Fields |
|---------|-----------------|---------|-----------------|
| MCP | `https://api.example.com/mcp` | `2025-06-18` | `mcpTools` array |
| A2A | `https://api.example.com/a2a` | `0.3.0` | `a2aSkills` array |
| ENS | `dataanalyst.eth` | `v1` | — |
| DID | `did:ethr:0x...` | `v1` | — |
| agentWallet | `eip155:1:0x...` (CAIP chain-qualified) | — | — |

Note: `agentWallet` is a service entry in the `services` array, not a top-level field.

## Rule 3: Capabilities via OASF

Use OASF (Open Agentic Schema Framework) to declare skills and domains.

- Schema: https://schema.oasf.outshift.com/0.8.0
- GitHub: https://github.com/agntcy/oasf/tree/v0.8.0
- From agntcy, now part of Linux Foundation

**Domains** are fields of application using path-style identifiers:
- `technology/software_engineering`
- `healthcare/telemedicine`
- `finance_and_business/investment_services`

**Skills** are specific capabilities using path-style identifiers:
- `natural_language_processing/natural_language_generation/summarization`
- `data_engineering/data_cleaning`

```json
{
  "name": "OASF",
  "endpoint": "https://github.com/agntcy/oasf/",
  "version": "v0.8.0",
  "skills": ["advanced_reasoning_planning/strategic_planning", "evaluation_monitoring/anomaly_detection"],
  "domains": ["finance_and_business/investment_services", "technology/blockchain/defi"]
}
```

## Rule 4: Registration Confirmation

Include a `registrations` array to confirm on-chain identity. `agentId` is a number, not a string:

```json
"registrations": [
  { "agentId": 42, "agentRegistry": "eip155:1:0x8004a6090Cd10A7288092483047B097295Fb8847" }
]
```

This links off-chain metadata to on-chain registration, enabling verification.

Optional: verify endpoint domain ownership via `.well-known/agent-registration.json`.

## Nice to Have

- `x402Support`: boolean indicating support for x402 payment protocol
- `active`: set to `false` until the agent is ready, then `true`
- `agentWallet`: can also be set on-chain via `setAgentWallet()`

## Production Example

```json
{
  "type": "https://eips.ethereum.org/EIPS/eip-8004#registration-v1",
  "name": "CryptoTrader Alpha",
  "description": "An autonomous AI trading agent that manages crypto portfolios on your behalf using advanced DeFi strategies. Supports automated trading on DEXs, yield optimization, risk management, and real-time market analysis. Trades on Ethereum, Base, Arbitrum, and Polygon. Features: portfolio rebalancing, stop-loss protection, MEV-resistant execution, gas optimization. Pricing: 2% performance fee on profits, no management fees. Minimum portfolio: 0.5 ETH. Audited smart contracts, non-custodial.",
  "image": "https://cdn.cryptotrader-alpha.io/logo.png",
  "services": [
    {
      "name": "MCP",
      "endpoint": "https://api.cryptotrader-alpha.io/mcp",
      "version": "2025-06-18",
      "mcpTools": ["portfolio_analysis", "execute_trade", "risk_assessment", "yield_optimizer", "market_sentiment_analysis", "gas_price_estimator", "defi_protocol_integrator"]
    },
    {
      "name": "OASF",
      "endpoint": "https://github.com/agntcy/oasf/",
      "version": "v0.8.0",
      "skills": ["advanced_reasoning_planning/strategic_planning", "evaluation_monitoring/anomaly_detection", "security_privacy/vulnerability_analysis"],
      "domains": ["finance_and_business/investment_services", "technology/blockchain/defi", "technology/blockchain/smart_contracts"]
    },
    {
      "name": "agentWallet",
      "endpoint": "eip155:1:0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb7"
    }
  ],
  "registrations": [
    {
      "agentId": 42,
      "agentRegistry": "eip155:1:0x8004a6090Cd10A7288092483047B097295Fb8847"
    }
  ],
  "supportedTrust": ["reputation", "crypto-economic"],
  "active": true,
  "x402Support": true
}
```

## Checklist

- [ ] Name is clear, memorable, and descriptive
- [ ] Image is high-quality (PNG, SVG, or WebP)
- [ ] Description covers what it does, how it works, and pricing if applicable
- [ ] All service endpoints are reachable
- [ ] agentWallet uses CAIP format (`eip155:<chainId>:<address>`)
- [ ] OASF skills/domains use path-style identifiers matching actual capabilities
- [ ] OASF references "Open Agentic Schema Framework" (correct full name)
- [ ] `registrations` array has numeric `agentId` matching on-chain registry entry
- [ ] `active` reflects current operational status
- [ ] `.well-known/agent-registration.json` set up for domain verification (optional)
