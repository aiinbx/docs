# Pacing API

Use the AI Inbx pacing API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/pacing)

Control send rate, hours, and queue flow.

## Endpoints

| Method | Operation | Path |
| --- | --- | --- |
| `GET` | [List pacing rules](endpoints/pacing/list-pacing-rules.md) | `/pacing-rules` |
| `POST` | [Create a pacing rule](endpoints/pacing/create-pacing-rule.md) | `/pacing-rules` |
| `GET` | [Retrieve a pacing rule](endpoints/pacing/retrieve-pacing-rule.md) | `/pacing-rules/{rule_id}` |
| `DELETE` | [Delete a pacing rule](endpoints/pacing/delete-pacing-rule.md) | `/pacing-rules/{rule_id}` |
| `PATCH` | [Update a pacing rule](endpoints/pacing/update-pacing-rule.md) | `/pacing-rules/{rule_id}` |
| `GET` | [Retrieve pacing spread](endpoints/pacing/retrieve-pacing-spread.md) | `/pacing-rules/spread` |
| `PATCH` | [Update pacing spread](endpoints/pacing/update-pacing-spread.md) | `/pacing-rules/spread` |
| `GET` | [Retrieve the pacing queue](endpoints/pacing/retrieve-pacing-queue.md) | `/pacing-queue` |
| `POST` | [Release emails from the pacing queue](endpoints/pacing/release-pacing-queue.md) | `/pacing-queue/release` |

## Request essentials [#request-essentials]

Use `https://api.aiinbx.com/api/v2` as the base URL and send your API key in `Authorization: Bearer <key>`. Keep credentials on your server.

See [Authentication](../authentication.md) for scopes, [Conventions](../reference/conventions.md) for pagination and idempotency, and [Errors](../reference/errors.md) for recovery guidance. Each endpoint page includes its parameters, response schemas, and copyable TypeScript SDK, Python SDK, and curl examples.
