# A2A auth and tenant

When the agent card declares security requirements, the SDK applies credentials to A2A requests. Use **MessageA2AOptions.credential**, **ListTasksOptions.credential**, or **LoadTaskOptions.credential** (or the task's credential when using the task handle) to pass auth.

## Agent card auth

The card can include **securitySchemes** (OpenAPI-style) and **security** (which schemes are required). Supported scheme types (per A2A spec §2.5):

- **apiKey** — `in`: header, query, or cookie; `name`: parameter name. Value is sent as API key.
- **http** — `scheme`: bearer or basic; optional `bearerFormat`. Value is sent as Bearer token or Basic auth.

The SDK matches the first available scheme and applies the credential. **CredentialObject** can be `{ apiKey: "..." }` or `{ bearer: "..." }`; a string is treated as `{ apiKey: string }`.

## Passing credentials

**TypeScript** — Options accept `credential?: string | CredentialObject`:

```ts
await agent.messageA2A('Hello', { credential: 'my-api-key' });
await agent.messageA2A('Hello', { credential: { bearer: 'token' } });
await agent.listTasks({ credential: 'my-api-key' });
await agent.loadTask(taskId, { credential: 'my-api-key' });
```

**Python** — Same idea; options take `credential` (str or dict):

```python
agent.messageA2A("Hello", MessageA2AOptions(credential="my-api-key"))
agent.listTasks(ListTasksOptions(credential="my-api-key"))
agent.loadTask(task_id, LoadTaskOptions(credential="my-api-key"))
```

When the card has no security or the endpoint does not require auth, credential can be omitted.

## Tenant

Some A2A endpoints are multi-tenant. The agent card can specify **tenant** on an interface. The SDK sends tenant when the server expects it (e.g. in headers or path). Callers do not usually set tenant; it comes from the resolved agent card. If your deployment uses tenant-specific URLs or headers, ensure the card (or discovery metadata) includes the tenant for that interface.

## Summary

- Use **credential** in message/task options when the agent's A2A endpoint requires API key or Bearer auth.
- Credential can be a string (→ apiKey) or object (`apiKey`, `bearer`). Security schemes on the card determine how it is applied.
- **Tenant** is read from the agent card; no separate caller option in the standard flow.
