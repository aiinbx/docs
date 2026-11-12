# MCP tools

Every tool the AI Inbx MCP servers offer, with its arguments and what a client is told about it before it runs.

[View on docs.aiinbx.com](https://docs.aiinbx.com/mcp/tools)

The workspace [MCP server](README.md), `aiinbx`, offers 25 tools for the daily work, and reaches the other 47 operations of the API through four generic tools. Each is an [API operation](../api/README.md) under another name, taking the same fields and answering with its JSON, trimmed for a model: `html` and `stripped_html` are left out where text says the same, an answer longer than 40,000 characters is cut with a note saying so, an empty success reads `Done.`, a failure is a tool error carrying the status, `code` and `detail`, and [outside content](#labels) comes wrapped in `<external_data>`.

Every tool also takes `workspace_id`. When a sign-in reaches more than one workspace, the assistant finds the ids with `aiinbx_workspaces_list` and names one on every call.

The [inbox server](../guides/agent-inbox.md), `aiinbx-inbox`, offers only the operations an inbox may call, `aiinbx_emails_send`, `aiinbx_emails_list`, `aiinbx_emails_retrieve`, `aiinbx_emails_update`, `aiinbx_emails_send_draft`, `aiinbx_emails_cancel`, `aiinbx_threads_list`, `aiinbx_threads_retrieve`, `aiinbx_threads_reply`, `aiinbx_threads_forward`, `aiinbx_workspaces_list` and `aiinbx_attachments_content`, and none of the four generic tools.

## Labels

Each tool tells the client what it does before it runs. Clients use this to decide what to ask you before calling it.

| Label | Meaning |
| --- | --- |
| Read-only | Changes nothing. |
| Writes | Changes something in the workspace. |
| Destructive | Takes away something a later call cannot put back. Clients ask before running it. |
| Reaches outside | Acts beyond the workspace: mail leaves, or a URL someone else runs is called. |
| Outside content | Answers with what somebody outside wrote: mail, subjects, attachment text. The answer says so, and the model reads instructions inside it as data. |

## Emails

### `aiinbx_emails_send`

Writes Reaches outside

Sends an email — or, with `draft: true`, writes it for a person to review and send. Requires a `from` address on a verified domain or a connected mailbox, and at least one of `html` or `text`. `thread_id` continues a conversation; to answer a message AI Inbx has not seen — mail from before a migration, or from another provider — name it in `in_reply_to` and `references` instead. Recipients on a matching suppression list are dropped and reported in `suppressed`; a pacing rule that holds the message is reported in `pacing`. Pass `Idempotency-Key` so a retry after a timeout replays the original send instead of sending twice. A workspace with no plan fails with 402 `plan_required`, and one that has used its plan's emails for the month with 402 `allowance_reached`.

| Prop | Type | Default | Description |
| - | - | - | - |
| `idempotency_key?` | `string` | - | Makes a send safe to retry. Reuse only for the same request body. |
| `from?` | `string \| object` | - | The sender, `you@yourdomain.com` or `Name <you@yourdomain.com>`, on a verified domain or a connected mailbox. Required, except from a connection made an inbox: left out, it sends as its inbox on the thread in `thread_id` — the one mail on the thread last went through, on a tie the one the thread is filed under — or as its only one. With several and none of them on the thread, it has to be named. |
| `to` | `string \| string[]` | - | Recipients: one address or a list, at most 100. |
| `subject` | `string` | - | The subject line. |
| `thread_id?` | `string` | - | Continue this thread: the email answers its latest message, and `In-Reply-To` and `References` are written from it. |
| `in_reply_to?` | `string` | - | The Message-ID this email answers, with or without angle brackets — for a reply to a message AI Inbx may never have seen. When it names an email of yours the send joins that thread; otherwise it opens one. Not with `thread_id`. |
| `references?` | `string[]` | - | The Message-IDs of the conversation so far, oldest first, written as `References`. `in_reply_to` is added at the end when it is not already there, and stands alone when this is left out. Not with `thread_id`. |
| `cc?` | `string \| string[]` | - | Cc recipients: one address or a list, at most 100. |
| `bcc?` | `string \| string[]` | - | Bcc recipients, hidden from the others: one address or a list, at most 100. |
| `reply_to?` | `string \| string[]` | - | Where replies go (Reply-To), when not to the sender: one address or a list, at most 100. |
| `attachments?` | `object[]` | - | Files to attach, at most 20 and 3 MB decoded in total: `filename`, `content_type` and `content` in base64, and a `cid` to make one an inline image. |
| `headers?` | `object` | - | Extra headers to write on the message. `Message-ID`, `In-Reply-To` and `References` are the API's to write and are refused. |
| `suppression_key?` | `string` | - | The suppression list the send is checked against, besides the workspace-wide `*` list. |
| `pacing?` | `object` | - | The send's say over pacing: `skip: true` goes past every rule, `count: false` uses up none of their limits. |
| `tracking?` | `object` | - | Overrides the sending domain's tracking settings for this email. Only mail sent from a domain is tracked: `true` on a send from a connected mailbox is refused with `tracking_unavailable`. |
| `scheduled_at?` | `string` | - | When to send, as an ISO 8601 time, at most 30 days out. On a draft, the time proposed for it. |
| `draft?` | `boolean` | - | Keep it as a draft for a person to review: nothing is sent until it is sent with `POST /emails/{id}/send` or from the console, at `draft.review_url`. A draft uses none of the plan's emails until then. |
| `html?` | `string` | - | The HTML body. |
| `text?` | `string` | - | The plain-text body. |
| `unsubscribe?` | `boolean` | - | Marks the email non-essential: it carries one-click unsubscribe headers, and `optional` suppressions apply to it. One recipient only. |

`POST /emails` · [API reference](../api/endpoints/emails/send-email.md)

### `aiinbx_emails_list`

Read-only Outside content

Lists emails in the workspace, newest first. Filter by `direction`, delivery `status`, a single `thread_id`, or a literal `query`: every word in it must appear in the subject, the sender, a recipient or the Message-ID, in any order and case. The query is a literal filter; the API does not provide semantic email search. To sync, walk forward instead: `order=asc` with `after` set to the `created_at` of the last email you have lists only what is new, oldest first.

| Prop | Type | Default | Description |
| - | - | - | - |
| `limit?` | `integer` | - | Maximum number of resources to return. |
| `cursor?` | `string` | - | Opaque cursor from the previous page. |
| `space?` | `string` | - | Only resources in this space. Pass `none` for resources that belong to the workspace itself (`space_id: null`), or omit for the workspace and all its spaces. |
| `after?` | `string` | - | Only what was created after this instant — `created_at` later than it, the instant itself left out. ISO 8601 with an offset. |
| `before?` | `string` | - | Only what was created before this instant — `created_at` earlier than it, the instant itself left out. ISO 8601 with an offset. |
| `order?` | `"asc" \| "desc"` | - | Which way the list is walked by `created_at`: `desc` is newest first, `asc` oldest first — the order a sync reads in. A cursor is a position, not a direction: send the same `order` with it. |
| `direction?` | `"inbound" \| "outbound"` | - | Filter by message direction. |
| `status?` | `"draft" \| "scheduled" \| "canceled" \| "discarded" \| "queued" \| "sending" \| "sent" \| "delivered" \| "bounced" \| "complained" \| "failed" \| "received"` | - | Filter by delivery status. |
| `thread_id?` | `string` | - | Filter to one thread. |
| `query?` | `string` | - | Words to find in the subject, sender, recipients or Message-ID. Every word must match. |

`GET /emails` · [API reference](../api/endpoints/emails/list-emails.md)

### `aiinbx_emails_retrieve`

Read-only Outside content

Returns one email in full: both bodies, every header, attachments, the delivery event history, and tracked engagements. Pass `include=attachment_content` to inline each attachment's prepared text, which saves a request per attachment before handing a message to a model.

| Prop | Type | Default | Description |
| - | - | - | - |
| `email_id` | `string` | - | The prefixed email ID. |
| `include?` | `"attachment_content"[]` | - | Expand optional content. Pass attachment_content to include prepared attachment text. |

`GET /emails/{email_id}` · [API reference](../api/endpoints/emails/retrieve-email.md)

### `aiinbx_emails_update`

Writes Reaches outside

Changes an email that has not gone out — a `draft` or a `scheduled` one: its recipients, subject, bodies, attachments or send time. Fields left out stay as they are. The email is composed again when it goes, so what is sent is what it says by then. One that has begun sending, or was canceled or discarded, returns 409 `not_scheduled`.

| Prop | Type | Default | Description |
| - | - | - | - |
| `email_id` | `string` | - | The prefixed email ID. |
| `to?` | `string \| string[]` | - | Recipients: one address or a list, at most 100. |
| `cc?` | `string \| string[]` | - | Cc recipients: one address or a list, at most 100. An empty list clears it. |
| `bcc?` | `string \| string[]` | - | Bcc recipients: one address or a list, at most 100. An empty list clears it. |
| `reply_to?` | `string \| string[]` | - | Where replies go (Reply-To): one address or a list, at most 100. An empty list clears it. |
| `subject?` | `string` | - | The subject line. |
| `html?` | `string \| null` | - | The HTML body. Null drops it, leaving the text. The text part is not rewritten from it — send `text` too to keep the two alike. |
| `text?` | `string \| null` | - | The plain-text body. Null drops it, leaving the HTML. |
| `attachments?` | `object[]` | - | The files it carries from now on: `{ id }` keeps one it has, a new one is given as on send. Left out, they stay; an empty list removes them all. New files decode to at most 3 MB per request, and all the email carries to at most 7 MB. |
| `scheduled_at?` | `string` | - | The new send time, at most 30 days out — on a draft, the time proposed for it. |

`PATCH /emails/{email_id}` · [API reference](../api/endpoints/emails/update-email.md)

### `aiinbx_emails_send_draft`

Writes Reaches outside

Sends a draft: at the `scheduled_at` given, else at the draft's own while it is still ahead, else now. From here it is a `scheduled` email — counted against the plan, and composed again when it goes. Anything but a draft returns 409 `not_draft`; a workspace out of emails returns 402.

| Prop | Type | Default | Description |
| - | - | - | - |
| `email_id` | `string` | - | The prefixed email ID. |
| `scheduled_at?` | `string` | - | When to send it, at most 30 days out. Left out, the time proposed on the draft while it is ahead, or now. |

`POST /emails/{email_id}/send` · [API reference](../api/endpoints/emails/send-draft.md)

### `aiinbx_emails_cancel`

Destructive

Cancels a scheduled email before it is composed and sent — its status becomes `canceled` — or discards a draft, which becomes `discarded`. Either is terminal: the email cannot be put back into flight.

| Prop | Type | Default | Description |
| - | - | - | - |
| `email_id` | `string` | - | The prefixed email ID. |

`POST /emails/{email_id}/cancel` · [API reference](../api/endpoints/emails/cancel-email.md)

## Threads

### `aiinbx_threads_list`

Read-only Outside content

Lists conversations, most recently active first. Every word of `query` must appear in the subject, the mailbox or a participant's address, literally and in any order; `mailbox` narrows to one address.

| Prop | Type | Default | Description |
| - | - | - | - |
| `limit?` | `integer` | - | Maximum number of resources to return. |
| `cursor?` | `string` | - | Opaque cursor from the previous page. |
| `space?` | `string` | - | Only resources in this space. Pass `none` for resources that belong to the workspace itself (`space_id: null`), or omit for the workspace and all its spaces. |
| `query?` | `string` | - | Words to find in the subject, mailbox or participants. Every word must match. |
| `mailbox?` | `string` | - | Filter by mailbox address. |

`GET /threads` · [API reference](../api/endpoints/threads/list-threads.md)

### `aiinbx_threads_retrieve`

Read-only Outside content

Returns a thread with its messages inline, oldest first. Messages paginate independently of the thread list via `message_limit` and `message_cursor`, so a long conversation can be walked without loading it whole.

| Prop | Type | Default | Description |
| - | - | - | - |
| `thread_id` | `string` | - | The prefixed thread ID. |
| `message_limit?` | `integer` | - | Maximum messages to return. |
| `message_cursor?` | `string` | - | Continue after this email from the previous page. |

`GET /threads/{thread_id}` · [API reference](../api/endpoints/threads/retrieve-thread.md)

### `aiinbx_threads_reply`

Writes Reaches outside

Replies within a thread. The sender, recipients, subject, and the `In-Reply-To` and `References` headers are inferred from the conversation, as a mail client infers them from the email answered: `email_id`, or the thread's latest. The reply follows that email's Reply-To; one that names only your own addresses is refused with 400 `no_recipients` rather than guessed around. Only the content is required — set `reply_all` or the individual fields to override what is inferred. Suppression and pacing behave exactly as on a send; an `Idempotency-Key` is bound to the request, so a retry replays the reply even after the thread has moved on. So does the plan: 402 `plan_required` without one, 402 `allowance_reached` once its emails are used up.

| Prop | Type | Default | Description |
| - | - | - | - |
| `idempotency_key?` | `string` | - | Makes a send safe to retry. Reuse only for the same request body. |
| `thread_id` | `string` | - | The prefixed thread ID. |
| `from?` | `string \| object` | - | The sender, `you@yourdomain.com` or `Name <you@yourdomain.com>`. Defaults to the thread's own; from a connection made an inbox, to its inbox on the thread — the one mail on the thread last went through, on a tie the one the thread is filed under. With several and none of them on the thread, it has to be named. |
| `email_id?` | `string` | - | The email in the thread this answers. Its Reply-To, sender and recipients decide whom the reply goes to, and `In-Reply-To` names it — also when a draft or scheduled reply goes out later. Defaults to the thread's latest email. Pass the id from `email.received` so mail arriving meanwhile cannot change whom you answer. |
| `to?` | `string \| string[]` | - | Recipients in place of the ones the answered email implies — its Reply-To, else its sender, or when that email is yours, whom it went to: one address or a list, at most 100. |
| `reply_all?` | `boolean` | - | Answer everyone on the answered email, as a mail client's Reply all does: its Reply-To (else its sender) and its `to` go in `to`, its `cc` in `cc`. With a Reply-To set, the sender is left out, as in Gmail. Only the address the reply goes out as, and its mailbox's aliases, are left off; colleagues stay. When the email is yours, its `to` and `cc` again. A `cc` given is added to the inferred one; a `to` given replaces it. |
| `subject?` | `string` | - | The subject, in place of `Re:` and the thread's. |
| `cc?` | `string \| string[]` | - | Cc recipients: one address or a list, at most 100. |
| `bcc?` | `string \| string[]` | - | Bcc recipients, hidden from the others: one address or a list, at most 100. |
| `reply_to?` | `string \| string[]` | - | Where replies go (Reply-To), when not to the sender: one address or a list, at most 100. |
| `attachments?` | `object[]` | - | Files to attach, at most 20 and 3 MB decoded in total: `filename`, `content_type` and `content` in base64, and a `cid` to make one an inline image. |
| `headers?` | `object` | - | Extra headers to write on the message. `Message-ID`, `In-Reply-To` and `References` are the API's to write and are refused. |
| `suppression_key?` | `string` | - | The suppression list the send is checked against, besides the workspace-wide `*` list. |
| `pacing?` | `object` | - | The send's say over pacing: `skip: true` goes past every rule, `count: false` uses up none of their limits. |
| `tracking?` | `object` | - | Overrides the sending domain's tracking settings for this email. Only mail sent from a domain is tracked: `true` on a send from a connected mailbox is refused with `tracking_unavailable`. |
| `scheduled_at?` | `string` | - | When to send, as an ISO 8601 time, at most 30 days out. On a draft, the time proposed for it. |
| `draft?` | `boolean` | - | Keep it as a draft for a person to review: nothing is sent until it is sent with `POST /emails/{id}/send` or from the console, at `draft.review_url`. A draft uses none of the plan's emails until then. |
| `html?` | `string` | - | The HTML body. |
| `text?` | `string` | - | The plain-text body. |
| `unsubscribe?` | `boolean` | - | Marks the email non-essential: it carries one-click unsubscribe headers, and `optional` suppressions apply to it. One recipient only. |

`POST /threads/{thread_id}/reply` · [API reference](../api/endpoints/threads/reply-to-thread.md)

### `aiinbx_threads_forward`

Writes Reaches outside

Sends the whole thread as one email to recipients who were not on it. Every message is rendered into one transcript, oldest first, each turn as what its author wrote with quoted tails and repeated signatures removed; the thread's attachments ride along unless `include_attachments` is false, and `note` is placed above the transcript. Only `to` is required: the sender defaults to the thread's mailbox and the subject to `Fwd:` and the thread's. The forward opens a thread of its own, linked back through `forward_of`, so a later reply on the original still goes to its participants and replies to the forward land on the forward. Suppression, pacing, scheduling, and idempotency behave exactly as on a send. So does the plan: 402 `plan_required` without one, 402 `allowance_reached` once its emails are used up.

| Prop | Type | Default | Description |
| - | - | - | - |
| `idempotency_key?` | `string` | - | Makes a send safe to retry. Reuse only for the same request body. |
| `thread_id` | `string` | - | The prefixed thread ID. |
| `from?` | `string \| object` | - | The sender, `you@yourdomain.com` or `Name <you@yourdomain.com>`. Defaults to the thread's mailbox; from a connection made an inbox, to its inbox on the thread — the one mail on the thread last went through, on a tie the one the thread is filed under. With several and none of them on the thread, it has to be named. |
| `to` | `string \| string[]` | - | Recipients: one address or a list, at most 100. |
| `subject?` | `string` | - | The subject. Defaults to `Fwd:` and the thread's. |
| `note?` | `string` | - | Text placed above the forwarded conversation. |
| `include_attachments?` | `boolean` | - | Whether the thread's attachments go along. |
| `cc?` | `string \| string[]` | - | Cc recipients: one address or a list, at most 100. |
| `bcc?` | `string \| string[]` | - | Bcc recipients, hidden from the others: one address or a list, at most 100. |
| `reply_to?` | `string \| string[]` | - | Where replies go (Reply-To), when not to the sender: one address or a list, at most 100. |
| `attachments?` | `object[]` | - | Files to attach, at most 20 and 3 MB decoded in total: `filename`, `content_type` and `content` in base64, and a `cid` to make one an inline image. |
| `headers?` | `object` | - | Extra headers to write on the message. `Message-ID`, `In-Reply-To` and `References` are the API's to write and are refused. |
| `suppression_key?` | `string` | - | The suppression list the send is checked against, besides the workspace-wide `*` list. |
| `pacing?` | `object` | - | The send's say over pacing: `skip: true` goes past every rule, `count: false` uses up none of their limits. |
| `tracking?` | `object` | - | Overrides the sending domain's tracking settings for this email. Only mail sent from a domain is tracked: `true` on a send from a connected mailbox is refused with `tracking_unavailable`. |
| `scheduled_at?` | `string` | - | When to send, as an ISO 8601 time, at most 30 days out. On a draft, the time proposed for it. |
| `draft?` | `boolean` | - | Keep it as a draft for a person to review: nothing is sent until it is sent with `POST /emails/{id}/send` or from the console, at `draft.review_url`. A draft uses none of the plan's emails until then. |

`POST /threads/{thread_id}/forward` · [API reference](../api/endpoints/threads/forward-thread.md)

## Domains

### `aiinbx_domains_list`

Read-only

Lists the workspace's sending domains with their verification state and tracking settings.

| Prop | Type | Default | Description |
| - | - | - | - |
| `limit?` | `integer` | - | Maximum number of resources to return. |
| `cursor?` | `string` | - | Opaque cursor from the previous page. |
| `space?` | `string` | - | Only resources in this space. Pass `none` for resources that belong to the workspace itself (`space_id: null`), or omit for the workspace and all its spaces. |

`GET /domains` · [API reference](../api/endpoints/domains/list-domains.md)

### `aiinbx_domains_create`

Writes

Registers a sending domain and returns the DNS records to publish: SPF, DKIM, DMARC, a return path, and an MX record for receiving. `region` selects the regional mail infrastructure for this domain; it can be changed with `PATCH /domains/{domain_id}` until the domain is verified.

A wildcard — `*.acme.dev` — is verified once and covers every subdomain: the MX record goes at `*.acme.dev`, the rest at `acme.dev`, and the apex keeps whatever mail setup it has. A name one label under a wildcard of yours — `hi.acme.dev` under `*.acme.dev` — is created as a subdomain of it: no records, verified as soon as the wildcard is, in whatever `space_id` you give it. That is how a platform hands each customer an address of their own without touching DNS again. To give customers single addresses of one shared domain instead, assign each address to its customer's space with `POST /addresses`.

A domain name can be connected in several spaces and workspaces, but sending and receiving are enabled for the one whose DKIM key is published. To move them, add the domain where it should go and publish its DNS records. The next verification check moves them; existing mail stays where it was. Duplicate apex or wildcard identities within one space fail with `domain_taken`.

Every workspace already has one domain it did not add: `<slug>.aiinbx.app`, listed with `provided: true`. It is verified through our zone, has no records and cannot be deleted; a name under `aiinbx.app` cannot be added by hand and fails with `reserved_domain`.

A domain of its own counts against the plan's custom domains — a subdomain under a wildcard and the provided domain do not. With none left the request fails with 402 `allowance_reached`, and a workspace with no plan with 402 `plan_required`.

`track_opens` and `track_clicks` set the domain's `tracking`; what they leave out starts as the workspace's tracking defaults in the console say — off unless they are set there — or, on a subdomain, as its wildcard's. Change either afterwards with `PATCH /domains/{domain_id}`.

| Prop | Type | Default | Description |
| - | - | - | - |
| `name` | `string` | - | A domain, a wildcard (`*.acme.dev`), or one label under a wildcard you own (`hi.acme.dev`) — the last is created as a subdomain with no records. |
| `region?` | `"eu-central-1" \| "us-east-1"` | - | The region the domain's mail is handled in. |
| `space_id?` | `string` | - | Which space a created resource goes in. Omitted, the workspace itself. |
| `track_opens?` | `boolean` | - | Whether opens are tracked on mail sent from the domain. |
| `track_clicks?` | `boolean` | - | Whether links are rewritten to track clicks on mail sent from the domain. |

`POST /domains` · [API reference](../api/endpoints/domains/create-domain.md)

### `aiinbx_domains_retrieve`

Read-only

Returns one domain with its DNS records and the state of each — `pending`, `verified`, or `missing` — along with when each was last checked.

| Prop | Type | Default | Description |
| - | - | - | - |
| `domain_id` | `string` | - | The prefixed domain ID. |

`GET /domains/{domain_id}` · [API reference](../api/endpoints/domains/retrieve-domain.md)

### `aiinbx_domains_update`

Writes

Updates whether opens and clicks are tracked on mail sent from the domain. Applies to every send from it that does not say otherwise in `tracking`, from the next one to leave. At least one field is required.

| Prop | Type | Default | Description |
| - | - | - | - |
| `domain_id` | `string` | - | The prefixed domain ID. |
| `track_opens?` | `boolean` | - | Whether opens are tracked on mail sent from the domain. |
| `track_clicks?` | `boolean` | - | Whether links are rewritten to track clicks on mail sent from the domain. |
| `region?` | `"eu-central-1" \| "us-east-1"` | - | Move a domain that has not verified yet to another region. Its MX and return-path records change with it and start over as pending; DKIM does not. `409 region_fixed` once it is verified, or on a subdomain. |

`PATCH /domains/{domain_id}` · [API reference](../api/endpoints/domains/update-domain.md)

### `aiinbx_domains_delete`

Destructive

Deletes a domain and stops future sends from it. Remove the published DNS records afterwards. A wildcard's subdomains are deleted with it, in whatever space they sit; a subdomain is removed on its own, with nothing to unpublish. The provided domain cannot be deleted and fails with `domain_provided`.

| Prop | Type | Default | Description |
| - | - | - | - |
| `domain_id` | `string` | - | The prefixed domain ID. |

`DELETE /domains/{domain_id}` · [API reference](../api/endpoints/domains/delete-domain.md)

### `aiinbx_domains_verify`

Writes

Checks the domain's DNS records against live DNS now. `verified_at` stays null until the sending identity’s DKIM verification succeeds; inbound MX, SPF, and DMARC readiness are separate. DNS propagation usually takes minutes and occasionally hours, so poll at a sane interval or subscribe to the `domain.verified` webhook instead. Domains are also rechecked in the background: a verified domain whose DKIM record stops resolving gets `dkim_missing_since`, keeps sending, and `domain.at_risk` is sent; `domain.restored` follows if the record is back, and has held for an hour, by `stops_sending_at`, otherwise it loses `verified_at` and `domain.lost` is sent.

| Prop | Type | Default | Description |
| - | - | - | - |
| `domain_id` | `string` | - | The prefixed domain ID. |

`POST /domains/{domain_id}/verify` · [API reference](../api/endpoints/domains/verify-domain.md)

### `aiinbx_domains_diagnostics`

Read-only

Reads the domain's live DNS zone and reports what is wrong with it — a duplicate SPF record, a permissive policy, a missing return path. Each finding carries a severity, an explanation, the fix, and the records it was drawn from. Nothing is stored; the result always reflects DNS as it is right now.

| Prop | Type | Default | Description |
| - | - | - | - |
| `domain_id` | `string` | - | The prefixed domain ID. |

`GET /domains/{domain_id}/diagnostics` · [API reference](../api/endpoints/domains/retrieve-domain-diagnostics.md)

## Suppressions

### `aiinbx_suppressions_list`

Read-only

Lists suppression entries, filterable by list `key`, `reason`, or a substring of the `address`. `blocks` and `last_blocked_at` show how often each entry has actually stopped a send.

| Prop | Type | Default | Description |
| - | - | - | - |
| `limit?` | `integer` | - | Maximum number of resources to return. |
| `cursor?` | `string` | - | Opaque cursor from the previous page. |
| `space?` | `string` | - | Only resources in this space. Pass `none` for resources that belong to the workspace itself (`space_id: null`), or omit for the workspace and all its spaces. |
| `key?` | `string` | - | Filter by suppression list key. |
| `address?` | `string` | - | Search recipient addresses. |
| `reason?` | `"complaint" \| "bounce" \| "unsubscribe" \| "manual"` | - | Filter by suppression reason. |

`GET /suppressions` · [API reference](../api/endpoints/suppressions/list-suppressions.md)

### `aiinbx_suppressions_add`

Writes

Adds up to 1000 addresses to a suppression list. `key` defaults to the workspace-wide `*` list and `scope` to `all`; a `scope` of `optional` blocks only sends marked as unsubscribable, so a marketing opt-out does not stop a password reset. Adding an address that is already listed updates the existing entry rather than duplicating it, and never narrows a wider scope. `space_id` puts the entries on a space's list, which stops that space's sends alone; without it they stop every space's.

| Prop | Type | Default | Description |
| - | - | - | - |
| `addresses` | `string[]` | - | The addresses to suppress, at most 1,000. |
| `key?` | `string` | - | The list: a `suppression_key` your sends name, or `*` for the workspace-wide one. |
| `space_id?` | `string` | - | Which space a created resource goes in. Omitted, the workspace itself. |
| `scope?` | `"all" \| "optional"` | - | `all` stops every send; `optional` only sends marked `unsubscribe`. |
| `note?` | `string` | - | Why the addresses are listed, for your records. |

`POST /suppressions` · [API reference](../api/endpoints/suppressions/create-suppressions.md)

### `aiinbx_suppressions_retrieve`

Read-only

Returns one suppression entry, including how it was created and how many sends it has stopped.

| Prop | Type | Default | Description |
| - | - | - | - |
| `suppression_id` | `string` | - | The prefixed suppression ID. |

`GET /suppressions/{suppression_id}` · [API reference](../api/endpoints/suppressions/retrieve-suppression.md)

### `aiinbx_suppressions_remove`

Destructive

Removes a suppression entry, allowing sends to that address again. Removing a `bounce` or `complaint` entry means mailing an address that already rejected you or reported you as spam — do it only with specific evidence the address was fixed. `provider_suppressed` says whether SES itself still refuses the address, which removing the entry does not change.

| Prop | Type | Default | Description |
| - | - | - | - |
| `suppression_id` | `string` | - | The prefixed suppression ID. |

`DELETE /suppressions/{suppression_id}` · [API reference](../api/endpoints/suppressions/delete-suppression.md)

## Workspace

### `aiinbx_workspaces_list`

Read-only

Lists the workspaces the caller can act in, with its role in each. An API key reaches its own workspace alone. An OAuth token reaches every workspace it was granted, and a request with one that reaches several names the workspace it acts in with the `AIInbx-Workspace` header, set to an `id` from here; this list is the one call that needs no header. `mode` says whether the caller acts as the whole of its workspaces or as an inbox; a token connected as an inbox reaches the workspaces of its inboxes, and `inboxes` names the addresses it reads and sends as.

It takes no arguments beyond `workspace_id`.

`GET /workspaces` · [API reference](../api/endpoints/workspace/list-workspaces.md)

### `aiinbx_workspaces_retrieve`

Read-only

Returns the workspace the request acts in: its name, its slug, the tracking defaults a domain added from now on starts with, and whether mail outside spaces threads across addresses.

It takes no arguments beyond `workspace_id`.

`GET /workspace` · [API reference](../api/endpoints/workspace/retrieve-workspace.md)

### `aiinbx_workspaces_update`

Writes

Renames the workspace, changes its tracking defaults or switches `thread_across_addresses` for mail outside spaces; only the fields given change. A new name brings a new slug, which moves the console URLs with it (the provided domain keeps its name), so a workspace can be renamed once a month — sooner fails with 409 `rename_cooldown`. Tracking defaults apply to domains added from now on; each existing domain keeps its own. Admin only.

| Prop | Type | Default | Description |
| - | - | - | - |
| `name?` | `string` | - | The workspace's name. |
| `tracking?` | `object` | - | What a domain added from now on starts with. Only the fields given change; existing domains keep theirs. |
| `thread_across_addresses?` | `boolean` | - | Whether an email without reply headers that arrives outside spaces can join any matching conversation outside spaces. Off, the default, it joins only a conversation the address it arrived at already took part in: sent in, received in, or was the Reply-To of. An email with reply headers always joins the conversation it answers. Turn it on when several of your addresses write to the same contacts in the same conversations. |

`PATCH /workspace` · [API reference](../api/endpoints/workspace/update-workspace.md)

## Attachments

### `aiinbx_attachments_content`

Read-only Outside content

Redirects to a short-lived signed URL for the attachment's prepared text — Markdown where the document has structure, plain text otherwise. Returns 404 `not_prepared` when the file type has no text to extract or preparation failed.

| Prop | Type | Default | Description |
| - | - | - | - |
| `attachment_id` | `string` | - | The prefixed attachment ID. |

`GET /attachments/{attachment_id}/content` · [API reference](../api/endpoints/attachments/retrieve-attachment-content.md)

## Everything else

The rest of the API — spaces, API keys, mailboxes, OAuth apps, webhooks, events, pacing and members — goes through four tools, so a client does not load 47 more definitions into every turn:

| Tool | What it does | Labels |
| --- | --- | --- |
| `aiinbx_api_list` | Lists every AI Inbx API operation, with its method, path and summary. Pass query to narrow it by words. | Read-only |
| `aiinbx_api_describe` | What one operation does and the arguments it takes, as a JSON Schema — call it with aiinbx_api_read (GET) or aiinbx_api_write. | Read-only |
| `aiinbx_api_read` | Calls a GET operation from aiinbx_api_list with its arguments. | Read-only |
| `aiinbx_api_write` | Calls a POST, PATCH or DELETE operation from aiinbx_api_list with its arguments. Some delete or reach outside the workspace; aiinbx_api_describe says which. | Destructive Reaches outside |

`aiinbx_api_read` and `aiinbx_api_write` take the operation's name, for example `pacing_rules_create`, as `operation`, and its fields, `workspace_id` among them, as `arguments`. Only the arguments say which write it is, so `aiinbx_api_write` carries the labels of the worst one. A client that asks before destructive tools asks before every call of it.

### Spaces

| Operation | What it does | Labels | API |
| --- | --- | --- | --- |
| `spaces_list` | Lists the workspace's spaces, oldest first. | Read-only | [`GET`](../api/endpoints/spaces/list-spaces.md) |
| `spaces_create` | Creates a space — one per customer, typically. | Writes | [`POST`](../api/endpoints/spaces/create-space.md) |
| `spaces_retrieve` | Returns one space by id, with its name, the `external_id` you gave it and its `thread_across_addresses` switch. | Read-only | [`GET`](../api/endpoints/spaces/retrieve-space.md) |
| `spaces_update` | Changes the space's name, `external_id` or `thread_across_addresses`; only the fields given change, and `external_id: null` clears it. | Writes | [`PATCH`](../api/endpoints/spaces/update-space.md) |
| `spaces_delete` | Marks the space for deletion and starts its teardown in the background. | Destructive | [`DELETE`](../api/endpoints/spaces/delete-space.md) |
| `addresses_list` | Lists the addresses assigned to a space of their own, newest first. | Read-only | [`GET`](../api/endpoints/spaces/list-addresses.md) |
| `addresses_assign` | Assigns one address of a verified domain of the workspace to a space, whatever space its domain is in: one shared domain, `anna@agents.com` in one customer's space and `bob@agents.com` in another's, with no DNS change. | Writes | [`POST`](../api/endpoints/spaces/assign-address.md) |
| `addresses_remove` | Removes the assignment: from its next email on, the address is where its untagged address is assigned — `anna@agents.com` for `anna+orders@agents.com` — else in its domain's space again. | Destructive | [`DELETE`](../api/endpoints/spaces/remove-address.md) |

### API Keys

| Operation | What it does | Labels | API |
| --- | --- | --- | --- |
| `api_keys_list` | Lists the workspace's API keys, newest first. | Read-only | [`GET`](../api/endpoints/api-keys/list-api-keys.md) |
| `api_keys_create` | Creates an organization-wide API key and returns its secret exactly once, in the `key` field. | Writes | [`POST`](../api/endpoints/api-keys/create-api-key.md) |
| `api_keys_delete` | Deletes a key immediately. | Destructive | [`DELETE`](../api/endpoints/api-keys/delete-api-key.md) |

### Mailboxes

| Operation | What it does | Labels | API |
| --- | --- | --- | --- |
| `mailboxes_list` | Lists connected Gmail and Outlook mailboxes with their sync state. | Read-only | [`GET`](../api/endpoints/mailboxes/list-mailboxes.md) |
| `mailboxes_connect` | Creates a single-use, short-lived URL that walks a customer through authorizing their Gmail or Outlook mailbox. | Writes | [`POST`](../api/endpoints/mailboxes/connect-mailbox.md) |
| `mailboxes_retrieve` | Returns one mailbox, including its `state` and the provider's `state_reason` when there is one. | Read-only | [`GET`](../api/endpoints/mailboxes/retrieve-mailbox.md) |
| `mailboxes_disconnect` | Disconnects a mailbox, stopping sync and dropping its stored credentials. | Destructive | [`DELETE`](../api/endpoints/mailboxes/disconnect-mailbox.md) |
| `mailboxes_sync` | Queues a catch-up sync for a mailbox. | Writes Reaches outside | [`POST`](../api/endpoints/mailboxes/sync-mailbox.md) |

### OAuth Apps

| Operation | What it does | Labels | API |
| --- | --- | --- | --- |
| `oauth_apps_list` | Lists the workspace's own Google and Microsoft OAuth apps. | Read-only | [`GET`](../api/endpoints/oauth-apps/list-o-auth-apps.md) |
| `oauth_apps_create` | Registers your own OAuth app so the mailbox consent screen carries your product's name rather than AI Inbx. | Writes | [`POST`](../api/endpoints/oauth-apps/create-o-auth-app.md) |
| `oauth_apps_retrieve` | Returns one OAuth app. | Read-only | [`GET`](../api/endpoints/oauth-apps/retrieve-o-auth-app.md) |
| `oauth_apps_update` | Updates an OAuth app's credentials, branding or return URLs. | Writes | [`PATCH`](../api/endpoints/oauth-apps/update-o-auth-app.md) |
| `oauth_apps_delete` | Deletes an OAuth app. | Destructive | [`DELETE`](../api/endpoints/oauth-apps/delete-o-auth-app.md) |

### Webhooks

| Operation | What it does | Labels | API |
| --- | --- | --- | --- |
| `webhook_endpoints_list` | Lists the workspace's webhook endpoints with their subscriptions and routing rules. | Read-only | [`GET`](../api/endpoints/webhooks/list-webhook-endpoints.md) |
| `webhook_endpoints_create` | Registers an organization-wide endpoint and returns its signing secret exactly once, in the `secret` field: no later read shows it again. | Writes | [`POST`](../api/endpoints/webhooks/create-webhook-endpoint.md) |
| `webhook_endpoints_retrieve` | Returns one endpoint, including when a rotated previous secret stops being accepted. | Read-only | [`GET`](../api/endpoints/webhooks/retrieve-webhook-endpoint.md) |
| `webhook_endpoints_update` | Updates an endpoint's URL, subscriptions, routing rules, payload, or enabled state. | Writes | [`PATCH`](../api/endpoints/webhooks/update-webhook-endpoint.md) |
| `webhook_endpoints_delete` | Deletes an endpoint. | Destructive | [`DELETE`](../api/endpoints/webhooks/delete-webhook-endpoint.md) |
| `webhook_endpoints_rotate_secret` | Issues a new signing secret and returns it, once — the only way to get a secret after the endpoint was created. | Destructive | [`POST`](../api/endpoints/webhooks/rotate-webhook-secret.md) |
| `webhook_endpoints_test` | Delivers a synthetic event to the endpoint and reports what it actually did — status code, duration, error, and the first 2 KB of its response. | Writes Reaches outside | [`POST`](../api/endpoints/webhooks/test-webhook-endpoint.md) |
| `webhook_endpoints_list_deliveries` | Lists deliveries to an endpoint, filterable by state. | Read-only | [`GET`](../api/endpoints/webhooks/list-webhook-deliveries.md) |
| `webhook_endpoints_retry_deliveries` | Replays up to 100 deliveries. | Writes Reaches outside | [`POST`](../api/endpoints/webhooks/retry-webhook-deliveries.md) |

### Events

| Operation | What it does | Labels | API |
| --- | --- | --- | --- |
| `events_list` | Lists the workspace's events, newest first — every event, recorded when it happens, whether or not a webhook endpoint subscribes to its type. | Read-only Outside content | [`GET`](../api/endpoints/events/list-events.md) |
| `events_retrieve` | Returns one event by id — the `id` of the envelope a webhook delivered, which is also the `AIInbx-Event-ID` header and a delivery's `event_id`. | Read-only Outside content | [`GET`](../api/endpoints/events/retrieve-event.md) |

### Pacing

| Operation | What it does | Labels | API |
| --- | --- | --- | --- |
| `pacing_rules_list` | Lists the workspace's pacing rules, both sending-hours and rate-limit kinds. | Read-only | [`GET`](../api/endpoints/pacing/list-pacing-rules.md) |
| `pacing_rules_create` | Creates a pacing rule. | Writes | [`POST`](../api/endpoints/pacing/create-pacing-rule.md) |
| `pacing_rules_retrieve` | Returns one pacing rule with its match patterns and its schedule or limit. | Read-only | [`GET`](../api/endpoints/pacing/retrieve-pacing-rule.md) |
| `pacing_rules_update` | Updates a pacing rule. | Writes | [`PATCH`](../api/endpoints/pacing/update-pacing-rule.md) |
| `pacing_rules_delete` | Deletes a pacing rule. | Destructive | [`DELETE`](../api/endpoints/pacing/delete-pacing-rule.md) |
| `pacing_rules_retrieve_spread` | Returns the workspace's send-timing spread, a value from 0 to 100. | Read-only | [`GET`](../api/endpoints/pacing/retrieve-pacing-spread.md) |
| `pacing_rules_update_spread` | Sets how much randomness is added to send timing. | Writes | [`PATCH`](../api/endpoints/pacing/update-pacing-spread.md) |
| `pacing_retrieve` | Returns a snapshot of everything pacing is currently holding: a sample of queued messages, the rules in effect, and per-rule counts of how many messages each matched and how many it is holding. | Read-only | [`GET`](../api/endpoints/pacing/retrieve-pacing-queue.md) |
| `pacing_release` | Releases up to 500 held emails immediately. | Writes Reaches outside | [`POST`](../api/endpoints/pacing/release-pacing-queue.md) |

### Members

| Operation | What it does | Labels | API |
| --- | --- | --- | --- |
| `members_list` | Lists everyone in the workspace, earliest to join first, with their role and when they were last active. | Read-only | [`GET`](../api/endpoints/members/list-members.md) |
| `members_update` | Changes what a member may do: `admin` manages the workspace, its members and its keys; `member` works with mail. | Destructive | [`PATCH`](../api/endpoints/members/update-member.md) |
| `members_remove` | Takes a member's access away at once. | Destructive | [`DELETE`](../api/endpoints/members/remove-member.md) |
| `invitations_list` | Lists the invitations nobody has answered, newest first — lapsed ones too, past their `expires_at`: those can no longer be accepted, and inviting the address again sends a fresh one. | Read-only | [`GET`](../api/endpoints/members/list-invitations.md) |
| `invitations_create` | Mails an invitation to join the workspace, with a link that is good for seven days. | Writes Reaches outside | [`POST`](../api/endpoints/members/create-invitation.md) |
| `invitations_cancel` | Cancels an invitation that is waiting to be accepted; the link in the mail stops working. | Destructive | [`DELETE`](../api/endpoints/members/cancel-invitation.md) |
