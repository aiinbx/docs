# Domains API

Use the AI Inbx domains API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/domains)

Manage sending domains and DNS checks.

## Endpoints

| Method | Operation | Path |
| --- | --- | --- |
| `GET` | [List domains](endpoints/domains/list-domains.md) | `/domains` |
| `POST` | [Create a domain](endpoints/domains/create-domain.md) | `/domains` |
| `GET` | [Retrieve a domain](endpoints/domains/retrieve-domain.md) | `/domains/{domain_id}` |
| `DELETE` | [Delete a domain](endpoints/domains/delete-domain.md) | `/domains/{domain_id}` |
| `PATCH` | [Update domain tracking settings](endpoints/domains/update-domain.md) | `/domains/{domain_id}` |
| `POST` | [Verify domain DNS records now](endpoints/domains/verify-domain.md) | `/domains/{domain_id}/verify` |
| `GET` | [Diagnose domain DNS configuration](endpoints/domains/retrieve-domain-diagnostics.md) | `/domains/{domain_id}/diagnostics` |

## Request essentials [#request-essentials]

Use `https://api.aiinbx.com/api/v2` as the base URL and send your API key in `Authorization: Bearer <key>`. Keep credentials on your server.

See [Authentication](../authentication.md) for scopes, [Conventions](../reference/conventions.md) for pagination and idempotency, and [Errors](../reference/errors.md) for recovery guidance. Each endpoint page includes its parameters, response schemas, and copyable TypeScript SDK, Python SDK, and curl examples.
