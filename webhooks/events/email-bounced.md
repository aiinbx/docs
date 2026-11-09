# email.bounced

email.bounced fires when delivery fails for good, hard or soft. Each recipient comes with a cause, and a dead address is suppressed, so the next send to it never leaves.

[View on docs.aiinbx.com](https://docs.aiinbx.com/webhooks/events/email-bounced)

Delivery failed: the message did not reach at least one recipient, and nothing will try again. `recipients` names only those; one the message did reach is not on it.

An automatic answer that comes back as a bounce, such as an out-of-office, is about mail that arrived. It is not recorded: the email keeps its status and its timeline, and this event does not fire.

## Payload

Every field, with an example body, is in the [`email.bounced` reference](../../api/endpoints/webhook-events/email-bounced.md), generated from the API contract.

Three fields say what happened, and they answer different questions:

- `permanent` is the provider's label for the bounce: `true` for a hard one, which it gave up on for good, `false` for a soft one, which it retried until it stopped. It does not say the address is dead. A blocklist refusal is often hard too.
- `bounces[].cause` is why each recipient bounced, read from what the receiving server said.
- `bounces[].suppressed` is whether that address went on the workspace's `*` [suppression](../../guides/suppressions.md) list, so the next send to it never leaves. Only a dead address is suppressed: a hard bounce with `cause` `bad_address`, or one nothing explains. An address the sending provider refused without trying, because it bounced as dead before, is `bad_address` but not suppressed: the provider keeps refusing it on its own.

`bounces` has one entry per address in `recipients`, in the same order. An event recorded before the field existed, as [`GET /events`](../../api/endpoints/events/list-events.md) or a replay still returns it, has no `bounces`.

| `cause`        | What it means                                                                                                                     |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| `bad_address`  | The address does not exist, or no longer takes mail.                                                                              |
| `mailbox_full` | The mailbox exists and is over its quota.                                                                                         |
| `sender_auth`  | The receiving side would not take the mail as from your domain: DMARC, SPF or DKIM failed. Fix it in the domain's DNS records.    |
| `blocked`      | The receiving side's policy refused it: a blocklist, the sender's reputation, a recipient that only takes mail from some senders. |
| `content`      | The message itself was refused: its content, an attachment, its size.                                                             |
| `expired`      | The provider retried until it gave up.                                                                                            |
| `routing`      | There is no route to the recipient, or the mail looped.                                                                           |
| `unknown`      | None of these could be read from what was said.                                                                                   |

More causes may be added; treat one you do not know as `unknown`.

## Handling it

```ts
if (event.type === "email.bounced") {
  for (const bounce of event.data.bounces ?? []) {
    if (bounce.suppressed) await markUndeliverable(bounce.address)
    else if (bounce.cause === "sender_auth") await alertDomainOwner(event.data)
  }
}
```
A suppressed address is wrong — stop showing it as valid in your own UI. Any other bounce leaves the address alone: the next send to it goes out as usual.

The suppression goes on the workspace's `*` list whatever `space_id` the event carries — the address exists for nobody, so no [space](../../guides/spaces.md) should try it either.
