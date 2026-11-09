# Threads API

Use the AI Inbx threads API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/threads)

Read email conversations.

## Endpoints

| Method | Operation | Path |
| --- | --- | --- |
| `GET` | [List threads](endpoints/threads/list-threads.md) | `/threads` |
| `GET` | [Retrieve a thread](endpoints/threads/retrieve-thread.md) | `/threads/{thread_id}` |
| `POST` | [Reply in a thread](endpoints/threads/reply-to-thread.md) | `/threads/{thread_id}/reply` |
| `POST` | [Forward a thread](endpoints/threads/forward-thread.md) | `/threads/{thread_id}/forward` |

## Request essentials [#request-essentials]

Use `https://api.aiinbx.com/api/v2` as the base URL and send your API key in `Authorization: Bearer <key>`. Keep credentials on your server.

See [Authentication](../authentication.md) for scopes, [Conventions](../reference/conventions.md) for pagination and idempotency, and [Errors](../reference/errors.md) for recovery guidance. Each endpoint page includes its parameters, response schemas, and copyable TypeScript SDK, Python SDK, and curl examples.
