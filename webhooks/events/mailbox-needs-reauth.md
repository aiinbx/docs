# mailbox.needs_reauth

mailbox.needs_reauth fires when a mailbox's stored authorization stops working: revoked access, a password change, or an admin policy.

[View on docs.aiinbx.com](https://docs.aiinbx.com/webhooks/events/mailbox-needs-reauth)

The stored authorization stopped working: revoked access, a password change, an admin policy.

## Payload

Every field, with an example body, is in the [`mailbox.needs_reauth` reference](../../api/endpoints/webhook-events/mailbox-needs-reauth.md), generated from the API contract.

## Handling it

Prompt the customer to reconnect here — `ref` and `metadata` say which of your users that is, without a lookup. Sync has already stopped, and sends will fail with [`mailbox_inactive`](../../reference/errors.md#mailbox_inactive) until it's fixed.

This is the one mailbox event worth paging a human about — every hour it goes unhandled is an hour of that customer's mail silently not moving.
