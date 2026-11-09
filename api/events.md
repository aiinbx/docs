# Events API

Use the AI Inbx events API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/events)

Read back what happened in the workspace — the events webhooks carry, whether or not an endpoint heard them.

## Endpoints

| Method | Operation | Path |
| --- | --- | --- |
| `GET` | [List events](endpoints/events/list-events.md) | `/events` |
| `GET` | [Retrieve an event](endpoints/events/retrieve-event.md) | `/events/{event_id}` |

## Request essentials [#request-essentials]

Use `https://api.aiinbx.com/api/v2` as the base URL and send your API key in `Authorization: Bearer <key>`. Keep credentials on your server.

See [Authentication](../authentication.md) for scopes, [Conventions](../reference/conventions.md) for pagination and idempotency, and [Errors](../reference/errors.md) for recovery guidance. Each endpoint page includes its parameters, response schemas, and copyable TypeScript SDK, Python SDK, and curl examples.
