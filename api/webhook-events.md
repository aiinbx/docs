# Webhook events

Every webhook event AI Inbx sends: one page per event with its payload schema, the signature headers, the answer that settles it, and an example body.

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/webhook-events)

What an endpoint receives: one request per event, each signed, in the envelope every event shares.

## Events

| Event | Fires when |
| --- | --- |
| [`email.received`](endpoints/webhook-events/email-received.md) | Inbound mail arrived and was matched onto a thread: once per received message, and again when the same message reaches another address of yours later. |
| [`email.sent`](endpoints/webhook-events/email-sent.md) | A message was handed to the mail provider: acceptance, not delivery. |
| [`email.delivered`](endpoints/webhook-events/email-delivered.md) | The receiving server accepted the message, reported per recipient. |
| [`email.bounced`](endpoints/webhook-events/email-bounced.md) | Delivery to one or more recipients failed; a dead address is suppressed. |
| [`email.complained`](endpoints/webhook-events/email-complained.md) | A recipient marked the message as spam, which weighs on sending reputation. |
| [`email.failed`](endpoints/webhook-events/email-failed.md) | The mail provider refused the message before any delivery attempt. |
| [`email.unsubscribed`](endpoints/webhook-events/email-unsubscribed.md) | A recipient opted out by link, one-click header, or reply. |
| [`email.opened`](endpoints/webhook-events/email-opened.md) | A tracked open was recorded, with bot and privacy-proxy hits flagged. |
| [`email.clicked`](endpoints/webhook-events/email-clicked.md) | A tracked link was clicked, with bot and scanner hits flagged. |
| [`thread.created`](endpoints/webhook-events/thread-created.md) | A new conversation started: an inbound message, a send, or a forward. |
| [`domain.verified`](endpoints/webhook-events/domain-verified.md) | A domain's DKIM identity was verified, and the domain can send. A new domain verifies at the first check that finds its DKIM record; one that was lost, only once the record has been back in DNS for an hour. |
| [`domain.at_risk`](endpoints/webhook-events/domain-at-risk.md) | A verified domain's DKIM record is missing from DNS: two of its last three checks did not find it, even with one between that did. It still sends; if the record is not back, and held for an hour, by `stops_sending_at` (72 hours after the first miss) the domain is lost. Sent only for the domain that holds the records: a wildcard's subdomains get none of their own. |
| [`domain.restored`](endpoints/webhook-events/domain-restored.md) | A domain's missing DKIM record has been back in DNS for an hour, with every check finding it, before the domain was lost; it signs as before. Sent only for the domain that holds the records: a wildcard's subdomains get none of their own. |
| [`domain.lost`](endpoints/webhook-events/domain-lost.md) | A verified domain stopped authenticating: SES stopped signing with its key, or its DKIM record went missing from DNS and was not back for an hour within 72 hours. It can no longer send. A wildcard's subdomains each get their own. |
| [`mailbox.connected`](endpoints/webhook-events/mailbox-connected.md) | A customer finished authorizing a Gmail or Outlook mailbox. |
| [`mailbox.needs_reauth`](endpoints/webhook-events/mailbox-needs-reauth.md) | A mailbox's stored authorization stopped working. |
| [`mailbox.disconnected`](endpoints/webhook-events/mailbox-disconnected.md) | A mailbox was removed, by you or by the customer revoking access. |

Check each delivery's signature before trusting it: see [Verifying](../webhooks/verifying.md).
