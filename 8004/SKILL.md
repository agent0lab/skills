---
name: "8004"
description: Explain ERC-8004 trustless-agents standard details at protocol level, including identity, reputation, and validation registries, agent identifiers, registration and feedback schemas, and security boundaries. Use when requests ask how ERC-8004 works, what the spec requires, how registry functions and events behave, or what the official contracts and discussion currently indicate.
---

# 8004

## Overview

Answer ERC-8004 questions by separating normative standard requirements from implementation status and open ecosystem discussion.

## Workflow

1. Classify the request into:
- `standard semantics`
- `contract implementation status`
- `design rationale or debate`
2. Load only concept files needed from `references/`.
3. Answer with this structure when applicable:
- `Spec`
- `Implementation`
- `Open questions`
4. Use exact identifiers and field names; avoid aliases.
5. Add date-sensitive caveats for deployment and support statements.

## Guardrails

- Preserve non-goals: payment rails remain out of scope.
- Do not invent registries, fields, or method names.
- If sources disagree, report disagreement explicitly.
- Treat EIP text as normative baseline and repository behavior as implementation evidence.

## Reference Map

Load these as needed, one concept per file:

- `references/source-map.md`
- `references/identity-model.md`
- `references/agent-identifier.md`
- `references/identity-functions.md`
- `references/identity-events.md`
- `references/reputation-model.md`
- `references/reputation-functions.md`
- `references/validation-flow.md`
- `references/metadata-patterns.md`
- `references/trust-boundaries.md`
- `references/status-and-deployments.md`
- `references/eip-changelog.md`
- `references/discrepancy-rules.md`
- `references/answer-examples.md`
- `references/glossary.md`
- `references/registration-best-practices.md`
- `references/reputation-best-practices.md`

Reusable template:

- `assets/spec-answer-template.md`

## Related Skills

- `agent0` — For SDK implementation questions, TypeScript/Python workflows, and building on top of ERC-8004

## Canonical Sources

- EIP: https://eips.ethereum.org/EIPS/eip-8004
- Discussion: https://ethereum-magicians.org/t/erc-8004-trustless-agents/25098
- Contracts and supporting spec: https://github.com/erc-8004/erc-8004-contracts
