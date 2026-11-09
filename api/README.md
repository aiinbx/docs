# API reference

Explore the AI Inbx REST API by resource. Find individual endpoints, request and response schemas, and examples for every operation.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api)

**Base URL** · `https://api.aiinbx.com/api/v2`

New to AI Inbx? Follow the [quickstart](../quickstart.md) for a complete send-and-reply flow. For an existing integration, start with [authentication](../authentication.md), [request conventions](../reference/conventions.md), or [errors](../reference/errors.md).

## Email and conversations [#email]

**[Emails](emails.md)**

Send, retrieve, schedule, and cancel messages.

**[Threads](threads.md)**

Read conversations, reply, and forward a transcript.

**[Attachments](attachments.md)**

Download original files and prepared text.

## Sending identities [#identities]

**[Domains](domains.md)**

Configure domains, verify DNS, and diagnose setup.

**[Mailboxes](mailboxes.md)**

Connect and manage Gmail or Outlook accounts.

**[OAuth apps](oauth-apps.md)**

Manage the apps used for mailbox authorization.

## Platform and delivery [#platform]

**[Spaces](spaces.md)**

Group customer identities, messages, and controls.

**[Webhooks](webhooks.md)**

Register endpoints, inspect deliveries, and replay failures.

**[Events](events.md)**

Read back what happened, and catch up after missed webhooks.

**[Suppressions](suppressions.md)**

Manage recipient exclusions and subscription preferences.

**[Pacing](pacing.md)**

Set sending rules and inspect or release held mail.

**[Workspace](workspace.md)**

Rename the workspace and set tracking defaults for new domains.

**[Members](members.md)**

Invite people, change their roles, and remove them.

**[API keys](api-keys.md)**

Create scoped credentials and manage their lifecycle.

**[Webhook events](../webhooks/events/README.md)**

Understand event payloads and delivery outcomes.

## SDKs and tools [#tools]

Use the official [TypeScript](../sdks/typescript.md) or [Python](../sdks/python.md) SDK, or call the API over HTTP. The [OpenAPI document](https://api.aiinbx.com/api/v2/openapi.json) defines the wire contract. [Documentation for agents](../reference/agents.md) explains Markdown access and implementation conventions.

For a complete list across resources, see [All endpoints](endpoints/README.md).
