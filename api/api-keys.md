# API Keys API

Use the AI Inbx api keys API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/api-keys)

Create and delete credentials for API access.

## Endpoints

| Method | Operation | Path |
| --- | --- | --- |
| `GET` | [List API keys](endpoints/api-keys/list-api-keys.md) | `/api-keys` |
| `POST` | [Create an API key](endpoints/api-keys/create-api-key.md) | `/api-keys` |
| `DELETE` | [Delete an API key](endpoints/api-keys/delete-api-key.md) | `/api-keys/{key_id}` |

## Request essentials [#request-essentials]

Use `https://api.aiinbx.com/api/v2` as the base URL and send your API key in `Authorization: Bearer <key>`. Keep credentials on your server.

See [Authentication](../authentication.md) for scopes, [Conventions](../reference/conventions.md) for pagination and idempotency, and [Errors](../reference/errors.md) for recovery guidance. Each endpoint page includes its parameters, response schemas, and copyable TypeScript SDK, Python SDK, and curl examples.
