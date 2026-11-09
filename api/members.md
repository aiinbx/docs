# Members API

Use the AI Inbx members API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/members)

Who is in the workspace, what each of them may do, and the invitations waiting to be accepted. Admin only.

## Endpoints

| Method | Operation | Path |
| --- | --- | --- |
| `GET` | [List members](endpoints/members/list-members.md) | `/members` |
| `DELETE` | [Remove a member](endpoints/members/remove-member.md) | `/members/{member_id}` |
| `PATCH` | [Update a member](endpoints/members/update-member.md) | `/members/{member_id}` |
| `GET` | [List invitations](endpoints/members/list-invitations.md) | `/invitations` |
| `POST` | [Invite someone](endpoints/members/create-invitation.md) | `/invitations` |
| `DELETE` | [Cancel an invitation](endpoints/members/cancel-invitation.md) | `/invitations/{invitation_id}` |

## Request essentials [#request-essentials]

Use `https://api.aiinbx.com/api/v2` as the base URL and send your API key in `Authorization: Bearer <key>`. Keep credentials on your server.

See [Authentication](../authentication.md) for scopes, [Conventions](../reference/conventions.md) for pagination and idempotency, and [Errors](../reference/errors.md) for recovery guidance. Each endpoint page includes its parameters, response schemas, and copyable TypeScript SDK, Python SDK, and curl examples.
