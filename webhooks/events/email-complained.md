# email.complained

email.complained fires when a recipient marks your message as spam. Rarer than a bounce and far more serious for your sending reputation.

[View on docs.aiinbx.com](https://docs.aiinbx.com/webhooks/events/email-complained)

The recipient marked the message as spam. Rarer than a bounce and far more serious for reputation.

## Payload

Every field, with an example body, is in the [`email.complained` reference](../../api/endpoints/webhook-events/email-complained.md), generated from the API contract.

## Handling it

Treat a complaint as final. The address is suppressed automatically; never remove that entry to keep sending.

A rising complaint rate is what gets a sending domain throttled or blocked by the large mailbox providers, so it's worth alerting on rather than only logging.
