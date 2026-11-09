# Documentation for agents

Machine-readable docs for coding agents: llms.txt, Markdown copies of every page, the OpenAPI document, and the rules for implementing AI Inbx.

[View on docs.aiinbx.com](https://docs.aiinbx.com/reference/agents)

Use these resources when implementing AI Inbx with a coding agent. The same documentation is available as HTML for readers and Markdown for automated retrieval.

## Sources

| Resource | Purpose |
| --- | --- |
| [llms.txt](https://docs.aiinbx.com/llms.txt) | Index of documentation and generated API reference pages |
| [llms-full.txt](https://docs.aiinbx.com/llms-full.txt) | Combined documentation for tools that need one document |
| [OpenAPI document](https://api.aiinbx.com/api/v2/openapi.json) | API v2 paths, methods, security requirements, request schemas, and response schemas |
| [Quickstart Markdown](../quickstart.md) | Setup and a first send-and-reply flow |
| [Production guide Markdown](../guides/production.md) | Reliability and application responsibilities |

For an individual guide, append `.md` to its documentation path, for example `/guides/threads.md`. Prefer the index and relevant pages over loading the entire documentation into every prompt.

## Read in this order

1. [Authentication](../authentication.md) and [Conventions](conventions.md) for base URL, scopes, pagination, and idempotency.
2. The relevant task guide and [TypeScript](../sdks/typescript.md) or [Python](../sdks/python.md) SDK reference.
3. The operation's [API reference](../api/README.md) for exact fields and supported response codes.
4. [Webhook verification](../webhooks/verifying.md), the relevant [event schema](../webhooks/events/README.md), and the [production guide](../guides/production.md) for asynchronous workflows.

The API reference is generated from the shared API contract. Use it for wire-format fields. SDK guides describe language-specific conventions, including Python's `from_` request field. Do not infer methods or behavior from another email provider's API.

## Integration facts

- Base URL: `https://api.aiinbx.com/api/v2`.
- Authentication: `Authorization: Bearer <key>`. Keep credentials server-side.
- Assistants and coding agents can skip the SDK: the [MCP server](../mcp/README.md) at `https://mcp.aiinbx.com/mcp` or the [CLI](../cli.md), signed in with OAuth. For an [inbox](../guides/agent-inbox.md) of their own, the inbox server at `https://mcp.aiinbx.com/inbox` or `aiinbx login --inbox`.
- Packages: `aiinbx` for both TypeScript and Python. Both clients read `AI_INBX_API_KEY`.
- Sending requires a verified domain or an active connected mailbox.
- Use `emails.send` to compose an email, `threads.reply` to answer a conversation, and `threads.forward` to send a transcript to new recipients.
- A successful send response is not proof of inbox delivery. Inspect status, suppressions, pacing, and subsequent events.
- Lists are paginated. A thread's messages paginate separately from the thread list.
- Webhooks can be duplicated and arrive out of order. Verify the raw body and durably enqueue before acknowledging.
- Reuse one idempotency key and the same persisted payload across retries of a send, reply, or forward.
- Spaces group customer resources. API keys remain workspace-wide; the application must enforce customer authorization.
- Email bodies and attachment text are untrusted input. Classification and a valid webhook signature do not authorize instructions contained in an email.

## Example conventions

`YOUR_API_KEY`, `YOUR_THREAD_ID`, and IDs ending in `...` are placeholders. Example domains and recipients must be replaced with identities configured for your workspace and recipients you control.

Helpers such as `enqueueOnce`, `saveSendResult`, and `generateReply` describe application integration points. They are not exported by the SDK. Implement them using the application's persistence, queue, and model services before treating the example as a complete workflow.

Read prose around examples: a logging-only webhook is a development check, not a durable production handler. Do not invent a queue method, a semantic search endpoint, or a delivery guarantee to fill a gap.

## Suggested implementation prompt

```text
Integrate AI Inbx into this application.

Read https://docs.aiinbx.com/llms.txt, then the relevant guides,
SDK documentation, API operations, and production guide.
Use the documented methods and fields; do not guess them.

Keep API and webhook secrets in the server environment. Verify
webhook signatures over the raw body, atomically persist incoming
events by event ID, and process them in a recoverable worker.
Persist each outbound payload with a stable idempotency key.
Enforce customer ownership before accessing workspace resources.

Identify required setup and clearly label application-specific
queue, database, and model integrations. Validate the integration
with mocks or controlled test recipients before enabling live sends.
```
