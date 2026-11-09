# Attachments API

Use the AI Inbx attachments API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/attachments)

Download original and prepared attachment content.

## Endpoints

| Method | Operation | Path |
| --- | --- | --- |
| `GET` | [Download an attachment](endpoints/attachments/download-attachment.md) | `/attachments/{attachment_id}` |
| `HEAD` | [Inspect attachment metadata](endpoints/attachments/inspect-attachment.md) | `/attachments/{attachment_id}` |
| `GET` | [Download prepared attachment text](endpoints/attachments/retrieve-attachment-content.md) | `/attachments/{attachment_id}/content` |

## Request essentials [#request-essentials]

Use `https://api.aiinbx.com/api/v2` as the base URL and send your API key in `Authorization: Bearer <key>`. Keep credentials on your server.

See [Authentication](../authentication.md) for scopes, [Conventions](../reference/conventions.md) for pagination and idempotency, and [Errors](../reference/errors.md) for recovery guidance. Each endpoint page includes its parameters, response schemas, and copyable TypeScript SDK, Python SDK, and curl examples.
