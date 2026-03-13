# TypeScript: messaging and tasks (A2A)

SDK 1.7.0+. Use a loaded **Agent** or **A2AClientFromSummary** for A2A. When you have only an **AgentSummary** (e.g. from search), use `sdk.createA2AClient(summary)` to get a client that resolves the agent card lazily and exposes the same API.

## Sending a message

```ts
// With a loaded Agent
const agent = await sdk.loadAgent('84532:1298');
const result = await agent.messageA2A('Hello');
// result: MessageResponse | TaskResponse | A2APaymentRequired<...>

// With a summary (no need to load full agent)
const summary = await sdk.getAgent('84532:1298');
const client = sdk.createA2AClient(summary);
const result = await client.messageA2A('Hello');
```

**messageA2A(content, options?)** — Content can be a string or `{ parts: Part[] }`. Options: `blocking`, `contextId`, `taskId`, `credential`, `payment`, `acceptedOutputModes`, `historyLength`, etc. Returns **MessageResponse** (direct reply), **TaskResponse** (server created a task), or **A2APaymentRequired** (402; use x402Payment.pay() then you get message/task).

## Tasks

- **listTasks(options?)** — Returns **TaskSummary[]** or A2APaymentRequired. Options: `filter`, `historyLength`, `credential`, `payment`.
- **loadTask(taskId, options?)** — Returns **AgentTask** or A2APaymentRequired. Options: `credential`, `payment`. After pay() on 402, you get an AgentTask.

**AgentTask** handle: same as the `task` on TaskResponse.

- **task.query()** — Get task state, artifacts, messages.
- **task.message(content)** — Send a follow-up message.
- **task.cancel()** — Cancel the task.

## Types

- **MessageResponse** — `content?`, `parts?`, `contextId?`; no task.
- **TaskResponse** — has `task: AgentTask`, `taskId`, `contextId`; use `'task' in result` to discriminate.
- **TaskSummary** — `taskId`, `contextId`, `status?`, `messages?`.
- **MessageA2AOptions** — `blocking`, `contextId`, `taskId`, `credential`, `payment`, `acceptedOutputModes`, `historyLength`, `pushNotificationConfig`, `returnImmediately`.

## Example (from demo)

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
