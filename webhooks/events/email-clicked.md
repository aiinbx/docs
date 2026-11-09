# email.clicked

email.clicked fires when a recipient follows a tracked link, a far stronger engagement signal than an open. Bot and scanner hits are flagged.

[View on docs.aiinbx.com](https://docs.aiinbx.com/webhooks/events/email-clicked)

A tracked link was clicked. A much stronger signal than an open. Requires click tracking on the [domain](../../guides/domains.md#tracking-settings), or [on the send](../../guides/sending.md#tracking).

## Payload

Every field, with an example body, is in the [`email.clicked` reference](../../api/endpoints/webhook-events/email-clicked.md), generated from the API contract.

## Handling it

A click is a real human action in a way an open is not, so this is the event to attribute against. Security scanners do visit links before the recipient does, though — those arrive with `bot: true`, so attribute only the clicks where it is `false`. The reasons are the same as for [`email.opened`](email-opened.md#bot-detection).
