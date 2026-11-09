# email.unsubscribed

email.unsubscribed fires when a recipient opts out by link, one-click header or reply, naming the suppression list and scope it was written to.

[View on docs.aiinbx.com](https://docs.aiinbx.com/webhooks/events/email-unsubscribed)

The recipient opted out.

## Payload

Every field, with an example body, is in the [`email.unsubscribed` reference](../../api/endpoints/webhook-events/email-unsubscribed.md), generated from the API contract.

## Handling it

```ts
if (event.type === "email.unsubscribed") {
  await recordOptOut(event.data.address, event.data.key, event.data.scope)
}
```
Mirror this into your own preference UI so a customer who unsubscribed by email doesn't see themselves still subscribed in your app.

`scope: "optional"` means they blocked marketing but still want the receipts and password resets — don't collapse it to a single "unsubscribed" boolean.
