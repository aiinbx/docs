# Create a mailbox connection URL

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/mailboxes/connect-mailbox)

Creates a single-use, short-lived URL that walks a customer through authorizing their Gmail or Outlook mailbox. Redirect them to it rather than emailing it. Pass `ref` — your own identifier — and `metadata` for anything else you want kept with the mailbox; both are stored on it and carried on every `mailbox.*` webhook. On a reconnect each replaces what the mailbox had, and one left out leaves it alone. The `mailbox.connected` webhook — not the browser returning to `return_to` — is the authoritative signal that the mailbox is live. `return_to` may be any URL here, since the request is authenticated; a connect link opened from a browser is held to the app's `return_urls` instead. `space_id` files the mailbox in a space; the returned URL carries it, so a platform mints one link per customer server-side rather than handing out the bare hosted link. A workspace with no plan fails with 402 `plan_required`. A mailbox counts against the plan's mailboxes, but which one it will be is only known once the person has authorized: with none left, a mailbox the workspace does not already hold is refused then, and the refusal is reported in `error` on `return_to`. Reconnecting one that is `active` or `needs_reauth` takes nothing new.

`POST /mailboxes/connect`
