# Agent Instructions

## Repository Purpose

This repository contains agent skills for the ERC-8004 Trustless Agents ecosystem. Each top-level directory (`8004/`, `agent0/`) is a self-contained skill following the Amp skill structure.

## Ground Rules

- NEVER MAKE THINGS UP. If you don't know something, notify your human.
- Double-check everything to ensure it's valid and not made up.

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter (`name`, `description`) as its entry point.
- Reference files in `references/` are loaded lazily — one concept per file, kept concise.
- The `agents/` subdirectories contain platform-specific agent configs (e.g., `openai.yaml`).
- EIP text is the normative baseline. Contracts repo is implementation evidence. Forum threads are rationale/debate.

## When Editing Skills

- Keep `SKILL.md` under 500 lines.
- Keep individual reference files focused on a single concept.
- Use exact identifiers and field names from the spec — do not invent aliases.
- Mark version-sensitive content (SDK method names, package names, deployment addresses) with caveats.
- If sources disagree, report the disagreement explicitly rather than picking a side silently.

## When Answering ERC-8004 Questions

- Separate normative spec requirements from implementation status and open discussion.
- Include date-sensitive caveats for deployment and support claims.
- Payment rails are explicitly out of scope for ERC-8004.

## When Answering Agent0 Questions

- Do not mix TypeScript and Python APIs in one recipe.
- Always ask for network, chainId, RPC URL, and signer details before write workflows.
- Treat indexer/search data as eventually consistent with on-chain state.

## Cross-Skill References

- The `8004` skill covers protocol-level spec questions. The `agent0` skill covers SDK integration.
- If a user asks a spec question while in the agent0 context, reference the `8004` skill.
- If a user asks how to implement something while in the 8004 context, reference the `agent0` skill.
- Both skills share a common glossary of terms. Prefer spec-defined terms over informal aliases.
