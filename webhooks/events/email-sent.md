# email.sent

email.sent fires when a message is handed to the mail provider: acceptance, not delivery. A full endpoint gets the whole email with it.

[View on docs.aiinbx.com](https://docs.aiinbx.com/webhooks/events/email-sent)

The message was handed to the provider. Acceptance, not delivery.

## Payload

Every field, with an example body, is in the [`email.sent` reference](../../api/endpoints/webhook-events/email-sent.md), generated from the API contract.

## Handling it

This is the event to record in an outbox or an audit log. It is *not* evidence the message arrived — wait for [`email.delivered`](email-delivered.md) for that, and expect [`email.bounced`](email-bounced.md) instead when it didn't.

A [scheduled send](../../guides/scheduling.md) fires this at the moment it actually goes out, not when you scheduled it.

On an endpoint with [`payload: "full"`](../README.md#full-payloads), `data.email` is the email as it reads when the delivery is sent — the body that went out, its headers and attachments — so an outbox can be written from the event alone. Its `status` and delivery `events` are as of that moment, not final.
