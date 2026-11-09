# OAuth Apps API

Use the AI Inbx oauth apps API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/oauth-apps)

Manage customer-owned Google and Microsoft OAuth apps.

## Endpoints

| Method | Operation | Path |
| --- | --- | --- |
| `GET` | [List OAuth apps](endpoints/oauth-apps/list-o-auth-apps.md) | `/oauth-apps` |
| `POST` | [Create an OAuth app](endpoints/oauth-apps/create-o-auth-app.md) | `/oauth-apps` |
| `GET` | [Retrieve an OAuth app](endpoints/oauth-apps/retrieve-o-auth-app.md) | `/oauth-apps/{app_id}` |
| `DELETE` | [Delete an OAuth app](endpoints/oauth-apps/delete-o-auth-app.md) | `/oauth-apps/{app_id}` |
| `PATCH` | [Update an OAuth app](endpoints/oauth-apps/update-o-auth-app.md) | `/oauth-apps/{app_id}` |

## Request essentials [#request-essentials]

Use `https://api.aiinbx.com/api/v2` as the base URL and send your API key in `Authorization: Bearer <key>`. Keep credentials on your server.

See [Authentication](../authentication.md) for scopes, [Conventions](../reference/conventions.md) for pagination and idempotency, and [Errors](../reference/errors.md) for recovery guidance. Each endpoint page includes its parameters, response schemas, and copyable TypeScript SDK, Python SDK, and curl examples.
