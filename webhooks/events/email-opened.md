# email.opened

email.opened fires on a tracked open, with bot and privacy-proxy hits flagged. Requires open tracking on the domain or on the send.

[View on docs.aiinbx.com](https://docs.aiinbx.com/webhooks/events/email-opened)

A tracked open. Requires open tracking on the [domain](../../guides/domains.md#tracking-settings), or [on the send](../../guides/sending.md#tracking).

## Payload

Every field, with an example body, is in the [`email.opened` reference](../../api/endpoints/webhook-events/email-opened.md), generated from the API contract.

## Bot detection

Every hit is reported; `bot` says whether a person made it. Count and act on the ones where it is `false`.

| `bot_reason` | What it is |
| --- | --- |
| `privacy_proxy` | Apple Mail Privacy Protection, which fetches every image as the message lands. |
| `script` | An HTTP library that announces itself. |
| `known_bot` | A security scanner, link expander or sync daemon that names itself. |
| `stale_browser` | A browser build nobody runs any more, which is what scanners dress up as. |
| `too_fast` | Within seconds of delivery, sooner than a person could act. |

More reasons may be added, so treat one you do not know as a bot. Events created before 2026-09-18 carry neither field.

Detection is conservative: a missing or unfamiliar user-agent counts as a person, and so do Gmail's and Yahoo's image proxies, which fetch when the reader opens the message.

## Handling it

> [!WARNING]
> Opens are unreliable by construction. Privacy proxies prefetch tracking pixels, which reports opens that never happened, and clients that block images report none at all. `bot` filters out the fetches that give themselves away; what is left is useful in aggregate, not evidence about an individual.

Never branch application logic on a single open — "they haven't read it, resend" will misfire on both kinds of client. [`email.clicked`](email-clicked.md) is the signal worth acting on.
