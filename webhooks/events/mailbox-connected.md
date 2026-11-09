# mailbox.connected

mailbox.connected fires when a customer finishes authorizing a Gmail or Outlook mailbox, carrying your ref and metadata to match it to a user.

[View on docs.aiinbx.com](https://docs.aiinbx.com/webhooks/events/mailbox-connected)

A customer finished authorizing a Gmail or Outlook mailbox.

## Payload

Every field, with an example body, is in the [`mailbox.connected` reference](../../api/endpoints/webhook-events/mailbox-connected.md), generated from the API contract.

## Handling it

This — not the browser landing back on `return_to` — is the signal that the mailbox is live. The redirect happens whether or not the grant completed, and a customer who closes the tab never triggers it at all.

Match on `ref` — whatever you put on the connect link or passed to `mailboxes.connect` — to attach the mailbox to the right user in your database. Anything more you need at this point belongs in [`metadata`](../../guides/mailboxes.md#ref-and-metadata), not JSON-encoded into `ref`.

On a reconnect, `ref` and `metadata` are what the mailbox holds now: what the new link carried, or what was there already where it carried none.

**[Mailboxes](../../guides/mailboxes.md)**

The connect flow end to end, including your own OAuth app.
