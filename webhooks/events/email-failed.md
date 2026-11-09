# email.failed

email.failed fires when the mail provider refuses a message before any delivery attempt: nothing reached a recipient and nothing is retried.

[View on docs.aiinbx.com](https://docs.aiinbx.com/webhooks/events/email-failed)

The mail provider refused the message before any delivery attempt — a virus verdict, a policy, a rendering failure. Nothing reached any recipient, and nothing will be retried: the message is `failed`.

## Payload

Every field, with an example body, is in the [`email.failed` reference](../../api/endpoints/webhook-events/email-failed.md), generated from the API contract.

## Handling it

```ts
if (event.type === "email.failed") {
  await flagForReview(event.data.email_id, event.data.reason)
}
```
A refusal is about the message, not the address: no suppression is written, and the same recipient can be written to again once the content is fixed.
