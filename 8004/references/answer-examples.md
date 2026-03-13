# Answer Examples

## "What is ERC-8004?"

**Spec:** Standard for trustless agent identity, reputation, and validation registries. Built on ERC-721. Requires EIP-155, EIP-712, EIP-721, ERC-1271.
**Implementation:** Identity and Reputation registries deployed via CREATE2 at deterministic addresses across 20+ networks.
**Open:** Validation registry semantics still evolving with TEE community.

## "How do I register an agent?"

**Spec:** Call `register(agentURI, metadata)` on the Identity Registry. Returns a `uint256 agentId`. Three overloads exist: with URI+metadata, URI only, or no arguments.
**Implementation:** Mainnet Identity Registry at `0x8004A169FB4a3325136EB29fA0ceB6D2e539a432`. Testnet at `0x8004A818BFB912233c491871b3d84c89A494BD9e`.

## "Can I build with it now?"

Yes for identity and reputation workflows — contracts are deployed on Ethereum, Base, Arbitrum, Optimism, Polygon, and more. Treat validation features as provisional. Verify deployment on your target chain.

## "Is this a payments standard?"

No. Payment rails are explicitly out of scope for ERC-8004. The `x402Support` field in registration files is an optional flag, not part of the core standard.

## "How does reputation work?"

**Spec:** Call `giveFeedback(agentId, value, valueDecimals, tag1, tag2, endpoint, feedbackURI, feedbackHash)`. Value is `int128` with `uint8` decimals (0–18). Self-feedback is prohibited. Use `getSummary()` with specific `clientAddresses` to avoid Sybil-vulnerable aggregation.

## "What is agentWallet?"

**Spec:** Reserved metadata key. Initially set to owner address. Changed via `setAgentWallet()` requiring EIP-712 or ERC-1271 signature from the new wallet. Cleared on ERC-721 transfer. Cannot be set via `setMetadata()`.
