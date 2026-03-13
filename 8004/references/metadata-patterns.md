# Metadata Patterns

## On-chain vs Off-chain Split

- **On-chain**: key-value pairs via `MetadataEntry` (high-trust, query-critical state)
- **Off-chain**: rich payloads stored at `agentURI` (registration file JSON)

## Registration File JSON Shape

The `agentURI` points to a JSON document with this structure:

```json
{
  "type": "https://eips.ethereum.org/EIPS/eip-8004#registration-v1",
  "name": "myAgentName",
  "description": "...",
  "image": "https://example.com/agentimage.png",
  "services": [
    { "name": "A2A", "endpoint": "https://...", "version": "0.3.0" },
    { "name": "MCP", "endpoint": "https://...", "version": "2025-06-18" },
    { "name": "OASF", "endpoint": "ipfs://{cid}", "version": "0.8", "skills": [], "domains": [] },
    { "name": "ENS", "endpoint": "vitalik.eth", "version": "v1" },
    { "name": "DID", "endpoint": "did:method:foobar", "version": "v1" }
  ],
  "x402Support": false,
  "active": true,
  "registrations": [{ "agentRegistry": "eip155:1:0x...", "agentId": 123 }],
  "supportedTrust": ["reputation", "crypto-economic", "tee-attestation"]
}
```

Key fields:
- `services`: array of protocol endpoints the agent supports (A2A, MCP, OASF, ENS, DID, etc.)
- `registrations`: all `{agentRegistry, agentId}` pairs for this agent across chains
- `supportedTrust`: trust mechanisms the agent participates in
- `x402Support`: whether the agent supports x402 payment protocol
