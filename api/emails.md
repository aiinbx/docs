# Emails API

Use the AI Inbx emails API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/emails)

Send, inspect, and schedule email.

## Endpoints

| Method | Operation | Path |
| --- | --- | --- |
| `GET` | [List emails](endpoints/emails/list-emails.md) | `/emails` |
| `POST` | [Send an email](endpoints/emails/send-email.md) | `/emails` |
| `GET` | [Retrieve an email](endpoints/emails/retrieve-email.md) | `/emails/{email_id}` |
| `PATCH` | [Update an email](endpoints/emails/update-email.md) | `/emails/{email_id}` |
| `POST` | [Send a draft](endpoints/emails/send-draft.md) | `/emails/{email_id}/send` |
| `POST` | [Cancel an email](endpoints/emails/cancel-email.md) | `/emails/{email_id}/cancel` |

## Request essentials [#request-essentials]

Use `https://api.aiinbx.com/api/v2` as the base URL and send your API key in `Authorization: Bearer <key>`. Keep credentials on your server.

See [Authentication](../authentication.md) for scopes, [Conventions](../reference/conventions.md) for pagination and idempotency, and [Errors](../reference/errors.md) for recovery guidance. Each endpoint page includes its parameters, response schemas, and copyable TypeScript SDK, Python SDK, and curl examples.
