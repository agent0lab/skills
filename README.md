# ERC-8004 Agent Skills

Agent skills for the [ERC-8004 Trustless Agents](https://eips.ethereum.org/EIPS/eip-8004) ecosystem.

## Skills

### `8004`

Explains ERC-8004 standard details at the protocol level — identity, reputation, and validation registries, agent identifiers, registration and feedback schemas, and security boundaries.

**Use when:** questions ask how ERC-8004 works, what the spec requires, how registry functions and events behave, or what the official contracts and discussion currently indicate.

### `agent0`

Integrates and operates [agent0lab](https://github.com/agent0lab) SDKs (`agent0-ts` and `agent0-py`) for ERC-8004 registration, capability advertisement (MCP, A2A, OASF), discovery/search, and reputation workflows.

**Use when:** requests ask for setup, code examples, architecture decisions, migration guidance, or troubleshooting for agent0 SDKs and companion services.

## Structure

```
8004/
├── SKILL.md              # Skill definition and workflow
├── agents/               # Agent platform configs
│   └── openai.yaml
└── references/           # Lazy-loaded reference docs
    ├── source-map.md
    ├── identity-model.md
    ├── identity-functions.md
    ├── identity-events.md
    ├── reputation-model.md
    ├── reputation-functions.md
    ├── validation-flow.md
    ├── metadata-patterns.md
    ├── trust-boundaries.md
    ├── status-and-deployments.md
    ├── discrepancy-rules.md
    ├── answer-examples.md
    ├── glossary.md
    ├── registration-best-practices.md
    └── reputation-best-practices.md

agent0/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── source-map.md
    ├── what-is-agent0.md
    ├── component-map.md
    ├── integration-decision-tree.md
    ├── env-prereqs.md
    ├── ts-install.md
    ├── ts-client-init.md
    ├── ts-registration.md
    ├── ts-search.md
    ├── ts-feedback.md
    ├── py-install.md
    ├── py-client-init.md
    ├── py-registration.md
    ├── py-search.md
    ├── py-feedback.md
    ├── production-rules.md
    ├── troubleshooting.md
    └── answer-examples.md
```

## Agent Platform Configs

The `agents/` subdirectory in each skill contains platform-specific configurations:

- `openai.yaml` — Configuration for OpenAI Agents platform

These are separate from the Amp skill definitions (`SKILL.md` + `references/`).

## Canonical Sources

- **EIP**: https://eips.ethereum.org/EIPS/eip-8004
- **Discussion**: https://ethereum-magicians.org/t/erc-8004-trustless-agents/25098
- **Contracts**: https://github.com/erc-8004/erc-8004-contracts
- **Agent0 Org**: https://github.com/agent0lab
- **Agent0 TS SDK**: https://github.com/agent0lab/agent0-ts
- **Agent0 Python SDK**: https://github.com/agent0lab/agent0-py
