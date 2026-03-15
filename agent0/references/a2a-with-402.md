# A2A with 402 Payment Required

An A2A server can respond with HTTP 402 Payment Required to **messageA2A**, **listTasks**, or **loadTask**. The SDK does not throw; it returns an **A2APaymentRequired** (or equivalent) so the caller can pay and continue.

## Shape of 402 response

When the server returns 402, the result has:

- **x402Required: true**
- **x402Payment** — Same pattern as generic x402: `accepts`, `pay(accept?)`, optional `payFirst()`.

After calling **x402Payment.pay()** (or **payFirst()**), the promise resolves to the same shape as success:

- For **messageA2A**: **MessageResponse** or **TaskResponse** (with optional `task` handle).
- For **listTasks**: **TaskSummary[]**.
- For **loadTask**: **AgentTask** (so you can then call `query()`, `message()`, `cancel()`).

## TypeScript

```ts
const result = await agent.messageA2A('Hello');
if (result.x402Required) {
  const paid = await result.x402Payment.pay(0);  // or payFirst()
  // paid is MessageResponse | TaskResponse
  if ('task' in paid) {
    const task = paid.task;
    await task.query();
  }
} else {
  // result is MessageResponse | TaskResponse directly
}

const tasksResult = await agent.listTasks();
if (tasksResult.x402Required) {
  const tasks = await tasksResult.x402Payment.pay();
  // tasks is TaskSummary[]
} else {
  const tasks = tasksResult;
}

const taskResult = await agent.loadTask(taskId);
if (taskResult.x402Required) {
  const task = await taskResult.x402Payment.pay();
  await task.query();
} else {
  const task = taskResult;
  await task.query();
}
```

## Python

Use **isX402Required(result)** or check `getattr(result, "x402Required", False)`. Then call **result.x402Payment.pay(0)** or **pay_first()**; the return value is the same as a normal success (message/task, list of TaskSummary, or AgentTask).

## Summary

- **messageA2A**, **listTasks**, and **loadTask** can return 402. Branch on **x402Required**; call **x402Payment.pay()** or **payFirst()**; then use the resolved value as the normal success type.
- For **loadTask**, after pay() you receive an **AgentTask** and can continue with query/message/cancel. Payment construction and retry follow the same pattern as ts-x402 / py-x402.
