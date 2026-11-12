# AI Inbx API

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints)

Email infrastructure for software that holds conversations: send, receive, thread and reply over one REST API, with a webhook for every event.

Version 2.0.0

Base URL: `https://api.aiinbx.com/api/v2`

## Spaces

Group a workspace's domains, mailboxes and mail by customer. Optional: a workspace that is not a platform never needs one.

- [`GET /spaces`](spaces/list-spaces.md) — List spaces.
- [`POST /spaces`](spaces/create-space.md) — Create a space.
- [`GET /spaces/{space_id}`](spaces/retrieve-space.md) — Retrieve a space.
- [`DELETE /spaces/{space_id}`](spaces/delete-space.md) — Delete a space.
- [`PATCH /spaces/{space_id}`](spaces/update-space.md) — Update a space.
- [`GET /addresses`](spaces/list-addresses.md) — List addresses.
- [`POST /addresses`](spaces/assign-address.md) — Assign an address to a space.
- [`DELETE /addresses/{address}`](spaces/remove-address.md) — Remove an address from its space.

## API Keys

Create and delete credentials for API access.

- [`GET /api-keys`](api-keys/list-api-keys.md) — List API keys.
- [`POST /api-keys`](api-keys/create-api-key.md) — Create an API key.
- [`DELETE /api-keys/{key_id}`](api-keys/delete-api-key.md) — Delete an API key.

## Emails

Send, inspect, and schedule email.

- [`GET /emails`](emails/list-emails.md) — List emails.
- [`POST /emails`](emails/send-email.md) — Send an email.
- [`GET /emails/{email_id}`](emails/retrieve-email.md) — Retrieve an email.
- [`PATCH /emails/{email_id}`](emails/update-email.md) — Update an email.
- [`POST /emails/{email_id}/send`](emails/send-draft.md) — Send a draft.
- [`POST /emails/{email_id}/cancel`](emails/cancel-email.md) — Cancel an email.

## Threads

Read email conversations.

- [`GET /threads`](threads/list-threads.md) — List threads.
- [`GET /threads/{thread_id}`](threads/retrieve-thread.md) — Retrieve a thread.
- [`POST /threads/{thread_id}/reply`](threads/reply-to-thread.md) — Reply in a thread.
- [`POST /threads/{thread_id}/forward`](threads/forward-thread.md) — Forward a thread.

## Attachments

Download original and prepared attachment content.

- [`GET /attachments/{attachment_id}`](attachments/download-attachment.md) — Download an attachment.
- [`HEAD /attachments/{attachment_id}`](attachments/inspect-attachment.md) — Inspect attachment metadata.
- [`GET /attachments/{attachment_id}/content`](attachments/retrieve-attachment-content.md) — Download prepared attachment text.

## Domains

Manage sending domains and DNS checks.

- [`GET /domains`](domains/list-domains.md) — List domains.
- [`POST /domains`](domains/create-domain.md) — Create a domain.
- [`GET /domains/{domain_id}`](domains/retrieve-domain.md) — Retrieve a domain.
- [`DELETE /domains/{domain_id}`](domains/delete-domain.md) — Delete a domain.
- [`PATCH /domains/{domain_id}`](domains/update-domain.md) — Update domain tracking settings.
- [`POST /domains/{domain_id}/verify`](domains/verify-domain.md) — Verify domain DNS records now.
- [`GET /domains/{domain_id}/diagnostics`](domains/retrieve-domain-diagnostics.md) — Diagnose domain DNS configuration.

## Mailboxes

Connect and manage provider mailboxes.

- [`GET /mailboxes`](mailboxes/list-mailboxes.md) — List mailboxes.
- [`POST /mailboxes/connect`](mailboxes/connect-mailbox.md) — Create a mailbox connection URL.
- [`GET /mailboxes/{mailbox_id}`](mailboxes/retrieve-mailbox.md) — Retrieve a mailbox.
- [`DELETE /mailboxes/{mailbox_id}`](mailboxes/disconnect-mailbox.md) — Disconnect a mailbox.
- [`POST /mailboxes/{mailbox_id}/sync`](mailboxes/sync-mailbox.md) — Queue a mailbox sync.

## OAuth Apps

Manage customer-owned Google and Microsoft OAuth apps.

- [`GET /oauth-apps`](oauth-apps/list-o-auth-apps.md) — List OAuth apps.
- [`POST /oauth-apps`](oauth-apps/create-o-auth-app.md) — Create an OAuth app.
- [`GET /oauth-apps/{app_id}`](oauth-apps/retrieve-o-auth-app.md) — Retrieve an OAuth app.
- [`DELETE /oauth-apps/{app_id}`](oauth-apps/delete-o-auth-app.md) — Delete an OAuth app.
- [`PATCH /oauth-apps/{app_id}`](oauth-apps/update-o-auth-app.md) — Update an OAuth app.

## Webhooks

Configure event delivery and inspect delivery attempts.

- [`GET /webhook-endpoints`](webhooks/list-webhook-endpoints.md) — List webhook endpoints.
- [`POST /webhook-endpoints`](webhooks/create-webhook-endpoint.md) — Create a webhook endpoint.
- [`GET /webhook-endpoints/{endpoint_id}`](webhooks/retrieve-webhook-endpoint.md) — Retrieve a webhook endpoint.
- [`DELETE /webhook-endpoints/{endpoint_id}`](webhooks/delete-webhook-endpoint.md) — Delete a webhook endpoint.
- [`PATCH /webhook-endpoints/{endpoint_id}`](webhooks/update-webhook-endpoint.md) — Update a webhook endpoint.
- [`POST /webhook-endpoints/{endpoint_id}/rotate-secret`](webhooks/rotate-webhook-secret.md) — Rotate a webhook signing secret.
- [`POST /webhook-endpoints/{endpoint_id}/test`](webhooks/test-webhook-endpoint.md) — Send a test webhook.
- [`GET /webhook-endpoints/{endpoint_id}/deliveries`](webhooks/list-webhook-deliveries.md) — List webhook deliveries.
- [`POST /webhook-endpoints/{endpoint_id}/deliveries/retry`](webhooks/retry-webhook-deliveries.md) — Retry webhook deliveries.

## Events

Read back what happened in the workspace — the events webhooks carry, whether or not an endpoint heard them.

- [`GET /events`](events/list-events.md) — List events.
- [`GET /events/{event_id}`](events/retrieve-event.md) — Retrieve an event.

## Webhook events

What an endpoint receives: one request per event, each signed, in the envelope every event shares.

- [`POST email.received`](webhook-events/email-received.md) — email.received.
- [`POST email.sent`](webhook-events/email-sent.md) — email.sent.
- [`POST email.delivered`](webhook-events/email-delivered.md) — email.delivered.
- [`POST email.bounced`](webhook-events/email-bounced.md) — email.bounced.
- [`POST email.complained`](webhook-events/email-complained.md) — email.complained.
- [`POST email.failed`](webhook-events/email-failed.md) — email.failed.
- [`POST email.unsubscribed`](webhook-events/email-unsubscribed.md) — email.unsubscribed.
- [`POST email.opened`](webhook-events/email-opened.md) — email.opened.
- [`POST email.clicked`](webhook-events/email-clicked.md) — email.clicked.
- [`POST thread.created`](webhook-events/thread-created.md) — thread.created.
- [`POST domain.verified`](webhook-events/domain-verified.md) — domain.verified.
- [`POST domain.at_risk`](webhook-events/domain-at-risk.md) — domain.at\_risk.
- [`POST domain.restored`](webhook-events/domain-restored.md) — domain.restored.
- [`POST domain.lost`](webhook-events/domain-lost.md) — domain.lost.
- [`POST mailbox.connected`](webhook-events/mailbox-connected.md) — mailbox.connected.
- [`POST mailbox.needs_reauth`](webhook-events/mailbox-needs-reauth.md) — mailbox.needs\_reauth.
- [`POST mailbox.disconnected`](webhook-events/mailbox-disconnected.md) — mailbox.disconnected.

## Suppressions

Prevent delivery to opted-out or unsafe recipients.

- [`GET /suppressions`](suppressions/list-suppressions.md) — List suppressions.
- [`POST /suppressions`](suppressions/create-suppressions.md) — Create suppressions.
- [`GET /suppressions/{suppression_id}`](suppressions/retrieve-suppression.md) — Retrieve a suppression.
- [`DELETE /suppressions/{suppression_id}`](suppressions/delete-suppression.md) — Delete a suppression.

## Pacing

Control send rate, hours, and queue flow.

- [`GET /pacing-rules`](pacing/list-pacing-rules.md) — List pacing rules.
- [`POST /pacing-rules`](pacing/create-pacing-rule.md) — Create a pacing rule.
- [`GET /pacing-rules/{rule_id}`](pacing/retrieve-pacing-rule.md) — Retrieve a pacing rule.
- [`DELETE /pacing-rules/{rule_id}`](pacing/delete-pacing-rule.md) — Delete a pacing rule.
- [`PATCH /pacing-rules/{rule_id}`](pacing/update-pacing-rule.md) — Update a pacing rule.
- [`GET /pacing-rules/spread`](pacing/retrieve-pacing-spread.md) — Retrieve pacing spread.
- [`PATCH /pacing-rules/spread`](pacing/update-pacing-spread.md) — Update pacing spread.
- [`GET /pacing-queue`](pacing/retrieve-pacing-queue.md) — Retrieve the pacing queue.
- [`POST /pacing-queue/release`](pacing/release-pacing-queue.md) — Release emails from the pacing queue.

## Workspace

The workspace a request acts in: its name, and the defaults new domains start with.

- [`GET /workspaces`](workspace/list-workspaces.md) — List workspaces.
- [`GET /workspace`](workspace/retrieve-workspace.md) — Retrieve the workspace.
- [`PATCH /workspace`](workspace/update-workspace.md) — Update the workspace.

## Members

Who is in the workspace, what each of them may do, and the invitations waiting to be accepted. Admin only.

- [`GET /members`](members/list-members.md) — List members.
- [`DELETE /members/{member_id}`](members/remove-member.md) — Remove a member.
- [`PATCH /members/{member_id}`](members/update-member.md) — Update a member.
- [`GET /invitations`](members/list-invitations.md) — List invitations.
- [`POST /invitations`](members/create-invitation.md) — Invite someone.
- [`DELETE /invitations/{invitation_id}`](members/cancel-invitation.md) — Cancel an invitation.
