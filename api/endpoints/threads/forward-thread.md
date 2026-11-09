# Forward a thread

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/threads/forward-thread)

Sends the whole thread as one email to recipients who were not on it. Every message is rendered into one transcript, oldest first, each turn as what its author wrote with quoted tails and repeated signatures removed; the thread's attachments ride along unless `include_attachments` is false, and `note` is placed above the transcript. Only `to` is required: the sender defaults to the thread's mailbox and the subject to `Fwd:` and the thread's. The forward opens a thread of its own, linked back through `forward_of`, so a later reply on the original still goes to its participants and replies to the forward land on the forward. Suppression, pacing, scheduling, and idempotency behave exactly as on a send. So does the plan: 402 `plan_required` without one, 402 `allowance_reached` once its emails are used up.

`POST /threads/{thread_id}/forward`
