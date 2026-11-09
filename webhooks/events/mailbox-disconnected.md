# mailbox.disconnected

mailbox.disconnected fires when a mailbox is removed, by you over the API or by the customer revoking access at Google or Microsoft.

[View on docs.aiinbx.com](https://docs.aiinbx.com/webhooks/events/mailbox-disconnected)

The mailbox was removed — by you, or by the customer revoking access at the provider.

## Payload

Every field, with an example body, is in the [`mailbox.disconnected` reference](../../api/endpoints/webhook-events/mailbox-disconnected.md), generated from the API contract.

## Handling it

Unlike [`mailbox.needs_reauth`](mailbox-needs-reauth.md), this is terminal — there is no authorization left to refresh. Clear the mailbox from your own UI rather than showing it as broken, and send the customer back through the connect flow if they want it again.
