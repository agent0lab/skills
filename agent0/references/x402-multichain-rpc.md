# Multichain and RPC (x402)

x402 payment options can reference different chains (e.g. Base, Base Sepolia). The SDK uses per-chain RPC URLs to build and sign payments on the correct chain.

## Default RPCs

SDK 1.7.0+ ships with built-in default RPC URLs for supported chains. Read-only and single-chain pay flows often need no override.

## Primary chain and overrides

- **Primary chain** — Set via `chainId` and `rpcUrl` (TS) or `chainId` and `rpcUrl` (Py) in SDK config.
- **overrideRpcUrls** — Map of chain id → RPC URL for other chains. Use when paying on a chain that is not the primary, or when defaults are wrong for your environment.

## TypeScript

```ts
const sdk = new SDK({
  chainId: 84532,  // Base Sepolia
  rpcUrl: process.env.RPC_URL!,
  signer: { privateKey: process.env.PRIVATE_KEY! },
  overrideRpcUrls: {
    8453: process.env.BASE_MAINNET_RPC_URL!,  // Base mainnet for payment option on that chain
  },
});
```

When a 402 response includes an accept with `network: "eip155:8453"`, the SDK uses `overrideRpcUrls[8453]` (or built-in default) to get a chain client and build the payment. If no RPC is available for that chain, the SDK throws with a message to add `overrideRpcUrls` for that chain id.

## Python

```python
sdk = SDK(
    chainId=84532,
    rpcUrl=os.environ["RPC_URL"],
    signer=private_key_or_signer,
    overrideRpcUrls={8453: os.environ.get("BASE_MAINNET_RPC_URL")},
)
```

Same idea: payment options with a different `network` use the matching RPC from defaults or `overrideRpcUrls`. The internal `get_web3_client_for_accept` (Py) / `getChainClientForAccept` (TS) resolves the chain and builds payment on the correct chain.

## Summary

- Add **overrideRpcUrls** when the server returns payment options on chains other than the SDK primary chain, or when you need to override default RPCs.
- Error message "payment option requires chain X but SDK is configured for chain Y" means add an RPC for chain X in **overrideRpcUrls** (or set primary chain if you only use one chain).
