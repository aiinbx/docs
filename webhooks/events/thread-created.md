# thread.created

thread.created fires when a new conversation starts: from an inbound message, a send that joined no thread, or a forward.

[View on docs.aiinbx.com](https://docs.aiinbx.com/webhooks/events/thread-created)

A new conversation started — by an inbound message, by a send that didn't join an existing thread, or by a [forward](../../guides/threads.md#forwarding).

## Payload

Every field, with an example body, is in the [`thread.created` reference](../../api/endpoints/webhook-events/thread-created.md), generated from the API contract.

## Handling it

Subscribe to this when you want to create a record — a ticket, a CRM entry — once per conversation rather than once per message. Every later message on the conversation reuses this `thread_id`, so the record never needs to be de-duplicated afterwards.

**[Threads](../../guides/threads.md)**

Reading a conversation, and replying or forwarding within it.
