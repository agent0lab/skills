# A2A interfaces and bindings

The SDK normalizes A2A interfaces from the agent card so the same client code works for different card shapes. The chosen interface determines the base URL and protocol binding used for message/task requests.

## Card shapes

- **v1 (supportedInterfaces)** — Card has `supportedInterfaces: [{ url, protocolBinding?, protocol?, protocolVersion?, tenant? }, ...]`. Each entry is one interface (URL + binding).
- **0.3 (url + additionalInterfaces)** — Card has `url` (primary) and optional `additionalInterfaces: [{ url, transport?, protocolVersion?, tenant? }, ...]`. `preferredTransport` on the card is the primary binding.

The SDK normalizes both to a list of **NormalizedInterface** (or equivalent): `url`, `binding`, `version`, `tenant?`.

## Bindings

- **HTTP+JSON** — HTTP POST with JSON body (typical REST-style A2A).
- **JSONRPC** — JSON-RPC over HTTP.
- **GRPC** — gRPC.
- **AUTO** — No binding declared; the client may try HTTP+JSON then JSON-RPC (implementation-specific).

Preferred order when multiple interfaces exist: HTTP+JSON, then JSONRPC, then GRPC, then AUTO. The SDK picks one interface and uses its URL and binding for `messageA2A`, `listTasks`, and `loadTask`.

## Picking an interface

The SDK uses **normalizeInterfaces(card)** to get the list and **pickInterface(interfaces, preferredBindings)** to choose one. Callers do not normally need to call these directly; Agent and A2AClientFromSummary resolve the interface when sending the first message or listing tasks. For custom or low-level use, the TS exports **normalizeInterfaces** and **pickInterface** from the A2A client module.

## Summary

- Agent card can be v1 (`supportedInterfaces`) or 0.3 (`url` + `additionalInterfaces`). SDK supports both.
- Binding order: HTTP+JSON preferred, then JSONRPC, GRPC, AUTO. One interface is chosen per agent for all A2A calls.
