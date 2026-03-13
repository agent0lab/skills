---
name: agent0
description: Integrate and operate agent0lab SDKs (agent0-ts and agent0-py) for ERC-8004 registration, capability advertisement (MCP, A2A, OASF), discovery/search, and reputation workflows. Use when requests ask for setup, code examples, architecture decisions, migration guidance, or troubleshooting for agent0 SDKs and companion services.
---

# Agent0

## Overview

Handle agent0 integration requests using short, runnable setup and flow guidance for both TypeScript and Python.

## Workflow

1. Classify request into:
- `orientation`
- `setup`
- `registration lifecycle`
- `search/discovery`
- `reputation feedback`
- `x402 (payment-required requests)`
- `a2a (agent-to-agent messaging and tasks)`
- `troubleshooting`
2. Load only relevant concept files from `references/`.
3. Ask for missing required inputs early:
- network and `chainId`
- RPC URL
- signer details for writes
- optional metadata storage details
4. Give output in runnable order:
- install
- initialize
- execute operation
- verify result
5. Mark assumptions:
- SDK version
- network
- read vs write permissions

## Guardrails

- Do not mix TypeScript and Python APIs in one recipe.
- Do not assume sample methods are exact for every SDK version.
- Do not skip network and signer validation for write workflows.
- Do not hide indexer lag or eventual consistency behavior.

## Reference Map

Load these as needed, one concept per file:

- `references/source-map.md`
- `references/what-is-agent0.md`
- `references/component-map.md`
- `references/integration-decision-tree.md`
- `references/env-prereqs.md`
- `references/ts-install.md`
- `references/ts-client-init.md`
- `references/ts-registration.md`
- `references/ts-search.md`
- `references/ts-feedback.md`
- `references/py-install.md`
- `references/py-client-init.md`
- `references/py-registration.md`
- `references/py-search.md`
- `references/py-feedback.md`
- `references/production-rules.md`
- `references/troubleshooting.md`
- `references/answer-examples.md`
- `references/x402-request-and-pay.md` — x402 request and pay (TS and Py)
- `references/x402-payment-on-first-request.md`
- `references/x402-multichain-rpc.md`
- `references/x402-v1-v2-eip3009.md`
- `references/a2a-messaging-and-tasks.md` — A2A messaging and tasks (TS and Py)
- `references/a2a-interfaces-bindings.md`
- `references/a2a-auth-tenant.md`
- `references/a2a-with-402.md`

Reusable templates:

- `assets/ts-recipe-template.md`
- `assets/py-recipe-template.md`
- Shared terms: `../8004/references/glossary.md`

## Related Skills

- `8004` — For ERC-8004 spec-level questions, normative requirements, and contract-level details

## Canonical Sources

- Organization: https://github.com/agent0lab
- TypeScript SDK: https://github.com/agent0lab/agent0-ts
- Python SDK: https://github.com/agent0lab/agent0-py
- SDK Docs: https://sdk.ag0.xyz
- Package: `agent0-sdk` on npm (ESM only) and PyPI
- Version: 1.7.0 at time of writing for x402/a2a; other reference files may reflect 1.5.3
