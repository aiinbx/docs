# Workspace API

Use the AI Inbx workspace API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/workspace)

The workspace a request acts in: its name, and the defaults new domains start with.

## Endpoints

| Method | Operation | Path |
| --- | --- | --- |
| `GET` | [List workspaces](endpoints/workspace/list-workspaces.md) | `/workspaces` |
| `GET` | [Retrieve the workspace](endpoints/workspace/retrieve-workspace.md) | `/workspace` |
| `PATCH` | [Update the workspace](endpoints/workspace/update-workspace.md) | `/workspace` |

## Request essentials [#request-essentials]

Use `https://api.aiinbx.com/api/v2` as the base URL and send your API key in `Authorization: Bearer <key>`. Keep credentials on your server.

See [Authentication](../authentication.md) for scopes, [Conventions](../reference/conventions.md) for pagination and idempotency, and [Errors](../reference/errors.md) for recovery guidance. Each endpoint page includes its parameters, response schemas, and copyable TypeScript SDK, Python SDK, and curl examples.
