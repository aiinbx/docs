# email.delivered

email.delivered fires when the receiving server accepts a message, per recipient. It does not prove inbox placement or that anyone read it.

[View on docs.aiinbx.com](https://docs.aiinbx.com/webhooks/events/email-delivered)

The receiving server accepted the message. This does not establish inbox placement or that the recipient read it.

## Payload

Every field, with an example body, is in the [`email.delivered` reference](../../api/endpoints/webhook-events/email-delivered.md), generated from the API contract.

## Handling it

A message to several recipients can produce several `email.delivered` events. Don't treat the first as "the message was delivered" — accumulate `recipients` across events if you need to know that everyone got it.

Accepted by the receiving server is as far as any email API can see. What happens after that — inbox, spam folder, a filter rule — is not observable from here.
