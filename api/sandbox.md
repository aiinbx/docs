# Sandbox API

Use the AI Inbx sandbox API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/sandbox)

Workspaces that run the whole API with simulated mail: no mail is delivered, none arrives from outside, and nothing is billed. Webhooks still go out, marked `sandbox: true`. Every new workspace starts as one. An agent can create one without an account and hand it to a person to claim.

## Endpoints

| Method | Operation | Path |
| --- | --- | --- |
| `POST` | [Create a sandbox](endpoints/sandbox/create-sandbox.md) | `/sandboxes` |
| `POST` | [Create a claim link](endpoints/sandbox/create-claim-link.md) | `/claim-links` |
| `POST` | [Simulate an inbound email](endpoints/sandbox/simulate-inbound-email.md) | `/sandbox/emails` |
| `POST` | [Simulate a reply](endpoints/sandbox/simulate-reply.md) | `/sandbox/emails/{email_id}/reply` |

## Request essentials [#request-essentials]

Use `https://api.aiinbx.com/api/v2` as the base URL and send your API key in `Authorization: Bearer <key>`. Keep credentials on your server.

See [Authentication](../authentication.md) for scopes, [Conventions](../reference/conventions.md) for pagination and idempotency, and [Errors](../reference/errors.md) for recovery guidance. Each endpoint page includes its parameters, response schemas, and copyable TypeScript SDK, Python SDK, and curl examples.
