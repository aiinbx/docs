# Suppressions API

Use the AI Inbx suppressions API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/suppressions)

Prevent delivery to opted-out or unsafe recipients.

## Endpoints

| Method | Operation | Path |
| --- | --- | --- |
| `GET` | [List suppressions](endpoints/suppressions/list-suppressions.md) | `/suppressions` |
| `POST` | [Create suppressions](endpoints/suppressions/create-suppressions.md) | `/suppressions` |
| `GET` | [Retrieve a suppression](endpoints/suppressions/retrieve-suppression.md) | `/suppressions/{suppression_id}` |
| `DELETE` | [Delete a suppression](endpoints/suppressions/delete-suppression.md) | `/suppressions/{suppression_id}` |

## Request essentials [#request-essentials]

Use `https://api.aiinbx.com/api/v2` as the base URL and send your API key in `Authorization: Bearer <key>`. Keep credentials on your server.

See [Authentication](../authentication.md) for scopes, [Conventions](../reference/conventions.md) for pagination and idempotency, and [Errors](../reference/errors.md) for recovery guidance. Each endpoint page includes its parameters, response schemas, and copyable TypeScript SDK, Python SDK, and curl examples.
