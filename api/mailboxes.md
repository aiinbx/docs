# Mailboxes API

Use the AI Inbx mailboxes API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/mailboxes)

Connect and manage provider mailboxes.

## Endpoints

| Method | Operation | Path |
| --- | --- | --- |
| `GET` | [List mailboxes](endpoints/mailboxes/list-mailboxes.md) | `/mailboxes` |
| `POST` | [Create a mailbox connection URL](endpoints/mailboxes/connect-mailbox.md) | `/mailboxes/connect` |
| `GET` | [Retrieve a mailbox](endpoints/mailboxes/retrieve-mailbox.md) | `/mailboxes/{mailbox_id}` |
| `DELETE` | [Disconnect a mailbox](endpoints/mailboxes/disconnect-mailbox.md) | `/mailboxes/{mailbox_id}` |
| `POST` | [Queue a mailbox sync](endpoints/mailboxes/sync-mailbox.md) | `/mailboxes/{mailbox_id}/sync` |

## Request essentials [#request-essentials]

Use `https://api.aiinbx.com/api/v2` as the base URL and send your API key in `Authorization: Bearer <key>`. Keep credentials on your server.

See [Authentication](../authentication.md) for scopes, [Conventions](../reference/conventions.md) for pagination and idempotency, and [Errors](../reference/errors.md) for recovery guidance. Each endpoint page includes its parameters, response schemas, and copyable TypeScript SDK, Python SDK, and curl examples.
