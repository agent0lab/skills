# Python: messaging and tasks (A2A)

SDK 1.7.0+. Use a loaded **Agent** or **A2AClientFromSummary** for A2A. When you have only an **AgentSummary** (e.g. from search), use `sdk.createA2AClient(summary)` to get a client that resolves the agent card lazily and exposes the same API.

## Sending a message

```python
# With a loaded Agent
agent = sdk.load_agent("84532:1298")
result = agent.messageA2A("Hello")
# result: MessageResponse | TaskResponse | A2APaymentRequired

# With a summary
summary = sdk.get_agent("84532:1298")
client = sdk.createA2AClient(summary)
result = client.messageA2A("Hello")
```

**messageA2A(content, options=None)** — Content: str or dict (e.g. `{"parts": [...]}`). Options: **MessageA2AOptions** (`blocking`, `contextId`, `taskId`, `credential`, `payment`, etc.). Returns **MessageResponse**, **TaskResponse**, or **A2APaymentRequired** (402; use x402Payment.pay() then you get message/task).

## Tasks

- **listTasks(options=None)** — Returns list of **TaskSummary** or A2APaymentRequired. Options: **ListTasksOptions** (`filter`, `historyLength`, `credential`, `payment`).
- **loadTask(task_id, options=None)** — Returns **AgentTask** or A2APaymentRequired. Options: **LoadTaskOptions** (`credential`, `payment`). After pay() on 402, you get an AgentTask.

**AgentTask** handle:

- **task.query(options=None)** — Get task state, artifacts, messages.
- **task.message(content)** — Send a follow-up message.
- **task.cancel()** — Cancel the task.

## Types (from agent0_sdk.core.a2a)

- **MessageResponse** — `content`, `parts`, `contextId`.
- **TaskResponse** — has `task` (AgentTask), `taskId`, `contextId`.
- **TaskSummary** — `taskId`, `contextId`, `status`, `messages`.
- **MessageA2AOptions**, **ListTasksOptions**, **LoadTaskOptions** — dataclasses with the option fields above.

## Example (from demo)

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
