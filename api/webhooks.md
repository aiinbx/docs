# Webhooks API

Use the AI Inbx webhooks API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/webhooks)

Configure event delivery and inspect delivery attempts.

## Endpoints

| Method | Operation | Path |
| --- | --- | --- |
| `GET` | [List webhook endpoints](endpoints/webhooks/list-webhook-endpoints.md) | `/webhook-endpoints` |
| `POST` | [Create a webhook endpoint](endpoints/webhooks/create-webhook-endpoint.md) | `/webhook-endpoints` |
| `GET` | [Retrieve a webhook endpoint](endpoints/webhooks/retrieve-webhook-endpoint.md) | `/webhook-endpoints/{endpoint_id}` |
| `DELETE` | [Delete a webhook endpoint](endpoints/webhooks/delete-webhook-endpoint.md) | `/webhook-endpoints/{endpoint_id}` |
| `PATCH` | [Update a webhook endpoint](endpoints/webhooks/update-webhook-endpoint.md) | `/webhook-endpoints/{endpoint_id}` |
| `POST` | [Rotate a webhook signing secret](endpoints/webhooks/rotate-webhook-secret.md) | `/webhook-endpoints/{endpoint_id}/rotate-secret` |
| `POST` | [Send a test webhook](endpoints/webhooks/test-webhook-endpoint.md) | `/webhook-endpoints/{endpoint_id}/test` |
| `GET` | [List webhook deliveries](endpoints/webhooks/list-webhook-deliveries.md) | `/webhook-endpoints/{endpoint_id}/deliveries` |
| `POST` | [Retry webhook deliveries](endpoints/webhooks/retry-webhook-deliveries.md) | `/webhook-endpoints/{endpoint_id}/deliveries/retry` |

## Request essentials [#request-essentials]

Use `https://api.aiinbx.com/api/v2` as the base URL and send your API key in `Authorization: Bearer <key>`. Keep credentials on your server.

See [Authentication](../authentication.md) for scopes, [Conventions](../reference/conventions.md) for pagination and idempotency, and [Errors](../reference/errors.md) for recovery guidance. Each endpoint page includes its parameters, response schemas, and copyable TypeScript SDK, Python SDK, and curl examples.
