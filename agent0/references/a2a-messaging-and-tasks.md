# A2A messaging and tasks

SDK 1.7.0+. Use a loaded **Agent** or **A2AClientFromSummary** for A2A. When you have only an **AgentSummary** (e.g. from search), use `sdk.createA2AClient(summary)` to get a client that resolves the agent card lazily and exposes the same API.

Do not mix TypeScript and Python in one recipe — use one language per flow.

## TypeScript

### Sending a message

```ts
// With a loaded Agent
const agent = await sdk.loadAgent('84532:1298');
const result = await agent.messageA2A('Hello');

// With a summary (no need to load full agent)
const summary = await sdk.getAgent('84532:1298');
const client = sdk.createA2AClient(summary);
const result = await client.messageA2A('Hello');
```

**messageA2A(content, options?)** — Content: string or `{ parts: Part[] }`. Options: `blocking`, `contextId`, `taskId`, `credential`, `payment`, `acceptedOutputModes`, `historyLength`, etc. Returns **MessageResponse**, **TaskResponse**, or **A2APaymentRequired** (402; use x402Payment.pay() then you get message/task).

### Tasks

- **listTasks(options?)** — Returns **TaskSummary[]** or A2APaymentRequired. Options: `filter`, `historyLength`, `credential`, `payment`.
- **loadTask(taskId, options?)** — Returns **AgentTask** or A2APaymentRequired. Options: `credential`, `payment`.

**AgentTask** handle (same as `task` on TaskResponse): **task.query()**, **task.message(content)**, **task.cancel()**.

### Types

- **MessageResponse** — `content?`, `parts?`, `contextId?`; no task.
- **TaskResponse** — has `task: AgentTask`, `taskId`, `contextId`; use `'task' in result` to discriminate.
- **TaskSummary** — `taskId`, `contextId`, `status?`, `messages?`.
- **MessageA2AOptions** — `blocking`, `contextId`, `taskId`, `credential`, `payment`, etc.

### Example

```ts
const agent = await sdk.loadAgent('84532:1298');
const msg = await agent.messageA2A('Hello, this is a demo message.');
if ('task' in msg) {
  const task = msg.task;
  await task.query();
  await task.message('Follow-up message.');
  await task.cancel();
}
const tasks = await agent.listTasks();
if (tasks.length > 0) {
  const task = await agent.loadTask(tasks[0].taskId);
  await task.query();
}
```

## Python

### Sending a message

```python
# With a loaded Agent
agent = sdk.load_agent("84532:1298")
result = agent.messageA2A("Hello")

# With a summary
summary = sdk.get_agent("84532:1298")
client = sdk.createA2AClient(summary)
result = client.messageA2A("Hello")
```

**messageA2A(content, options=None)** — Content: str or dict (e.g. `{"parts": [...]}`). Options: **MessageA2AOptions**. Returns **MessageResponse**, **TaskResponse**, or **A2APaymentRequired** (402; use x402Payment.pay() then you get message/task).

### Tasks

- **listTasks(options=None)** — Returns list of **TaskSummary** or A2APaymentRequired. Options: **ListTasksOptions** (`filter`, `historyLength`, `credential`, `payment`).
- **loadTask(task_id, options=None)** — Returns **AgentTask** or A2APaymentRequired. Options: **LoadTaskOptions** (`credential`, `payment`).

**AgentTask** handle: **task.query(options=None)**, **task.message(content)**, **task.cancel()**.

### Types (from agent0_sdk.core.a2a)

- **MessageResponse**, **TaskResponse**, **TaskSummary** — `content`, `parts`, `contextId`, `task`, `taskId`, `status`, `messages` as applicable.
- **MessageA2AOptions**, **ListTasksOptions**, **LoadTaskOptions** — dataclasses with the option fields.

### Example

```python
agent = sdk.load_agent("84532:1298")
result = agent.messageA2A("Hello, this is a demo message.")
if hasattr(result, "task") and result.task:
    task = result.task
    task.query()
    task.message("Follow-up message.")
    task.cancel()
tasks = agent.listTasks()
if tasks:
    task = agent.loadTask(tasks[0].taskId)
    task.query()
```
