# Spaces API

Use the AI Inbx spaces API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/spaces)

Group a workspace's domains, mailboxes and mail by customer. Optional: a workspace that is not a platform never needs one.

## Endpoints

| Method | Operation | Path |
| --- | --- | --- |
| `GET` | [List spaces](endpoints/spaces/list-spaces.md) | `/spaces` |
| `POST` | [Create a space](endpoints/spaces/create-space.md) | `/spaces` |
| `GET` | [Retrieve a space](endpoints/spaces/retrieve-space.md) | `/spaces/{space_id}` |
| `DELETE` | [Delete a space](endpoints/spaces/delete-space.md) | `/spaces/{space_id}` |
| `PATCH` | [Update a space](endpoints/spaces/update-space.md) | `/spaces/{space_id}` |
| `GET` | [List addresses](endpoints/spaces/list-addresses.md) | `/addresses` |
| `POST` | [Assign an address to a space](endpoints/spaces/assign-address.md) | `/addresses` |
| `DELETE` | [Remove an address from its space](endpoints/spaces/remove-address.md) | `/addresses/{address}` |

## Request essentials [#request-essentials]

Use `https://api.aiinbx.com/api/v2` as the base URL and send your API key in `Authorization: Bearer <key>`. Keep credentials on your server.

See [Authentication](../authentication.md) for scopes, [Conventions](../reference/conventions.md) for pagination and idempotency, and [Errors](../reference/errors.md) for recovery guidance. Each endpoint page includes its parameters, response schemas, and copyable TypeScript SDK, Python SDK, and curl examples.
