# undefined

Read the AI Inbx docs instead of answering from memory. Email infrastructure for software that holds conversations. Send, receive, thread, and reply over one API. Use when a task involves AI Inbx: how it works, how to set it up or configure it, or its API.

[View on docs.aiinbx.com](https://docs.aiinbx.com/skill)

# AI Inbx

Email infrastructure for software that holds conversations. Send, receive, thread, and reply over one API.

This skill points you at the AI Inbx docs, at https://docs.aiinbx.com/. When a task involves AI Inbx, read the relevant page before relying on what you remember: the docs are the source of truth, and they change.

## Reading the docs

- Any page as Markdown: add `.md` to its URL (the home page is https://docs.aiinbx.com/index.md). Every page link below already points at the Markdown.
- [llms.txt](https://docs.aiinbx.com/llms.txt): every page with a one-line summary.
- [llms-full.txt](https://docs.aiinbx.com/llms-full.txt): every page in one file, for when you need all of it.
- [JSON API](https://docs.aiinbx.com/api/docs/pages.json): the page index, with each page as JSON and Markdown.

## API reference

- [API Reference](https://docs.aiinbx.com/api/endpoints), with a page per operation (add `.md` for its Markdown).

## Pages

- [AI Inbx documentation](https://docs.aiinbx.com/index.md): Learn to send and receive email, manage conversation threads, and build AI email agents with the AI Inbx API, TypeScript SDK, and Python SDK.
- [Quickstart](https://docs.aiinbx.com/quickstart.md): Get started with AI Inbx: add a sending domain, send your first email with the SDK, and receive the reply through a verified webhook.
- [Core concepts](https://docs.aiinbx.com/concepts.md): The building blocks of AI Inbx: workspaces, sending identities, emails, threads, events, and the spaces that keep each customer's mail apart.
- [Authentication](https://docs.aiinbx.com/authentication.md): Authenticate with Bearer API keys: the three scopes and what each one reaches, how to keep keys server-side, and how to rotate and delete them.
- [Going to production](https://docs.aiinbx.com/guides/production.md): Go to production with AI Inbx: reliable, idempotent sends, durable webhook processing, customer boundaries, and monitoring of delivery outcomes.
- [Plans and billing](https://docs.aiinbx.com/guides/billing.md): What each plan includes, exactly what counts against it, how extra usage and spending caps work, and what the API answers at a limit.

### API reference

- [API reference](https://docs.aiinbx.com/api.md): Explore the AI Inbx REST API by resource. Find individual endpoints, request and response schemas, and examples for every operation.

#### Emails

- [Emails API](https://docs.aiinbx.com/api/emails.md): Use the AI Inbx emails API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

#### Threads

- [Threads API](https://docs.aiinbx.com/api/threads.md): Use the AI Inbx threads API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

#### Attachments

- [Attachments API](https://docs.aiinbx.com/api/attachments.md): Use the AI Inbx attachments API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

#### Domains

- [Domains API](https://docs.aiinbx.com/api/domains.md): Use the AI Inbx domains API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

#### Mailboxes

- [Mailboxes API](https://docs.aiinbx.com/api/mailboxes.md): Use the AI Inbx mailboxes API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

#### OAuth Apps

- [OAuth Apps API](https://docs.aiinbx.com/api/oauth-apps.md): Use the AI Inbx oauth apps API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

#### Spaces

- [Spaces API](https://docs.aiinbx.com/api/spaces.md): Use the AI Inbx spaces API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

#### Webhooks

- [Webhooks API](https://docs.aiinbx.com/api/webhooks.md): Use the AI Inbx webhooks API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

#### Events

- [Events API](https://docs.aiinbx.com/api/events.md): Use the AI Inbx events API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

#### Webhook events

- [Webhook events](https://docs.aiinbx.com/api/webhook-events.md): Every webhook event AI Inbx sends: one page per event with its payload schema, the signature headers, the answer that settles it, and an example body.

#### Suppressions

- [Suppressions API](https://docs.aiinbx.com/api/suppressions.md): Use the AI Inbx suppressions API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

#### Pacing

- [Pacing API](https://docs.aiinbx.com/api/pacing.md): Use the AI Inbx pacing API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

#### Workspace

- [Workspace API](https://docs.aiinbx.com/api/workspace.md): Use the AI Inbx workspace API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

#### Members

- [Members API](https://docs.aiinbx.com/api/members.md): Use the AI Inbx members API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

#### API Keys

- [API Keys API](https://docs.aiinbx.com/api/api-keys.md): Use the AI Inbx api keys API. Browse endpoints, request parameters, response schemas, and code examples for your integration.

### Sending

- [Sending](https://docs.aiinbx.com/guides/sending.md): Addresses, HTML and text bodies, attachments, custom headers, idempotency, and what the send response tells you.
- [Scheduling](https://docs.aiinbx.com/guides/scheduling.md): Schedule a send up to 30 days ahead, move it, or cancel it, and learn what stays editable on a scheduled email until the moment it goes out.
- [Drafts](https://docs.aiinbx.com/guides/drafts.md): Let an agent write an email and a person decide: create drafts over the API, then review, edit and send them from the console or your own UI.
- [Suppressions](https://docs.aiinbx.com/guides/suppressions.md): Lists that stop the next send — bounces, complaints, unsubscribes — scoped per campaign or across the whole workspace.
- [Pacing](https://docs.aiinbx.com/guides/pacing.md): Pace outbound mail with sending hours and rate ceilings, see the queue they hold messages in, and inspect, release or override held sends.

### Receiving

- [Receiving](https://docs.aiinbx.com/guides/receiving.md): Two ways mail reaches your code — an MX record on a domain, or a connected Gmail or Outlook mailbox — and what arrives when it does.
- [Threads](https://docs.aiinbx.com/guides/threads.md): Read a conversation, page through a long one, and reply or forward within a thread without reconstructing In-Reply-To or References headers.
- [Attachments](https://docs.aiinbx.com/guides/attachments.md): Download what arrived, or read the prepared Markdown of each attachment so a model can use a PDF or a document without you running a parser.

### Addresses

- [Domains](https://docs.aiinbx.com/guides/domains.md): Add a sending domain, publish the DNS records, verify it, diagnose the one record that won't resolve, and cover every subdomain at once with a wildcard.
- [Mailboxes](https://docs.aiinbx.com/guides/mailboxes.md): Connect existing Gmail or Outlook accounts over OAuth from a hosted page, a link or the API, keep them synced, and brand the consent screen.
- [Spaces](https://docs.aiinbx.com/guides/spaces.md): Group a workspace's domains, mailboxes and mail by customer — one space each — with rules and suppression lists that reach that space alone.

### Webhooks

- [Webhooks](https://docs.aiinbx.com/webhooks.md): Register an endpoint, choose events, route them by address, and understand what happens when a delivery fails.
- [Verifying](https://docs.aiinbx.com/webhooks/verifying.md): Verify AI Inbx webhook signatures: the HMAC-SHA256 scheme, verifying in the TypeScript and Python SDKs, and checking it by hand in any language.

#### Events

- [Event reference](https://docs.aiinbx.com/webhooks/events.md): Every AI Inbx webhook event type, the envelope they share, what each one means, and how to handle email, thread, domain and mailbox events.
- [email.received](https://docs.aiinbx.com/webhooks/events/email-received.md): email.received fires for every inbound message, already matched onto its thread, and names the addresses of yours it arrived at. The event most AI Inbx applications are built around.
- [email.sent](https://docs.aiinbx.com/webhooks/events/email-sent.md): email.sent fires when a message is handed to the mail provider: acceptance, not delivery. A full endpoint gets the whole email with it.
- [email.delivered](https://docs.aiinbx.com/webhooks/events/email-delivered.md): email.delivered fires when the receiving server accepts a message, per recipient. It does not prove inbox placement or that anyone read it.
- [email.bounced](https://docs.aiinbx.com/webhooks/events/email-bounced.md): email.bounced fires when delivery fails for good, hard or soft. Each recipient comes with a cause, and a dead address is suppressed, so the next send to it never leaves.
- [email.complained](https://docs.aiinbx.com/webhooks/events/email-complained.md): email.complained fires when a recipient marks your message as spam. Rarer than a bounce and far more serious for your sending reputation.
- [email.failed](https://docs.aiinbx.com/webhooks/events/email-failed.md): email.failed fires when the mail provider refuses a message before any delivery attempt: nothing reached a recipient and nothing is retried.
- [email.unsubscribed](https://docs.aiinbx.com/webhooks/events/email-unsubscribed.md): email.unsubscribed fires when a recipient opts out by link, one-click header or reply, naming the suppression list and scope it was written to.
- [email.opened](https://docs.aiinbx.com/webhooks/events/email-opened.md): email.opened fires on a tracked open, with bot and privacy-proxy hits flagged. Requires open tracking on the domain or on the send.
- [email.clicked](https://docs.aiinbx.com/webhooks/events/email-clicked.md): email.clicked fires when a recipient follows a tracked link, a far stronger engagement signal than an open. Bot and scanner hits are flagged.
- [thread.created](https://docs.aiinbx.com/webhooks/events/thread-created.md): thread.created fires when a new conversation starts: from an inbound message, a send that joined no thread, or a forward.
- [domain.verified](https://docs.aiinbx.com/webhooks/events/domain-verified.md): domain.verified fires when a domain's DKIM identity is verified and it can send. Use it to offer the domain as a sender in your product.
- [domain.at_risk](https://docs.aiinbx.com/webhooks/events/domain-at-risk.md): domain.at_risk fires when a verified domain's DKIM record goes missing from DNS. The domain still sends; if the record is not back by stops_sending_at, 72 hours later, the domain is lost.
- [domain.restored](https://docs.aiinbx.com/webhooks/events/domain-restored.md): domain.restored fires when a domain's missing DKIM record has been back in DNS for an hour before the domain was lost. The domain signs as before.
- [domain.lost](https://docs.aiinbx.com/webhooks/events/domain-lost.md): domain.lost fires when a verified domain's DKIM record has been missing from DNS for 72 hours: sends from it fail with domain_unverified until the record is back.
- [mailbox.connected](https://docs.aiinbx.com/webhooks/events/mailbox-connected.md): mailbox.connected fires when a customer finishes authorizing a Gmail or Outlook mailbox, carrying your ref and metadata to match it to a user.
- [mailbox.needs_reauth](https://docs.aiinbx.com/webhooks/events/mailbox-needs-reauth.md): mailbox.needs_reauth fires when a mailbox's stored authorization stops working: revoked access, a password change, or an admin policy.
- [mailbox.disconnected](https://docs.aiinbx.com/webhooks/events/mailbox-disconnected.md): mailbox.disconnected fires when a mailbox is removed, by you over the API or by the customer revoking access at Google or Microsoft.

### Integrate

- [Integrate AI Inbx](https://docs.aiinbx.com/integrations.md): Integrate AI Inbx with your stack: Next.js and Hono webhook handlers, React Email templates, and agent loops that send, read and reply to mail.
- [Next.js](https://docs.aiinbx.com/integrations/nextjs.md): Use AI Inbx in Next.js: a route handler that verifies and receives webhooks, and a server action that sends email with the TypeScript SDK.
- [Hono](https://docs.aiinbx.com/integrations/hono.md): Receive AI Inbx webhooks in Hono on Cloudflare Workers, Bun, Deno and Node: verify the signature over the raw body and hand the event to a worker.
- [React Email](https://docs.aiinbx.com/integrations/react-email.md): Send React Email templates through AI Inbx: pass a component as `react` and the TypeScript SDK renders its HTML. Keep passing `text` beside it.
- [Build an AI email agent](https://docs.aiinbx.com/integrations/agents.md): Build an AI email agent with signed webhooks, conversation threads, human review, and idempotent replies using the AI Inbx TypeScript SDK.

### MCP & CLI

- [MCP server](https://docs.aiinbx.com/mcp.md): Run AI Inbx from Claude, ChatGPT, Cursor, VS Code, Codex or any MCP client, or give it an inbox of its own: two servers, signed in as you or with an API key.
- [MCP tools](https://docs.aiinbx.com/mcp/tools.md): Every tool the AI Inbx MCP servers offer, with its arguments and what a client is told about it before it runs.
- [CLI](https://docs.aiinbx.com/cli.md): The aiinbx command line: send and read mail, call every API resource, forward webhooks to localhost without a tunnel, and check a domain's DNS.
- [Agent skills](https://docs.aiinbx.com/skills.md): Agent skills for AI Inbx: let a coding agent set up the CLI, sign in, wire sending and receiving into your app, and verify one email and its reply end to end.
- [Give your agent an inbox](https://docs.aiinbx.com/guides/agent-inbox.md): Connect Claude Code, Cursor or the aiinbx CLI to an inbox — an email address of its own that it sends from and reads, and nothing else in your workspace.
- [Documentation for agents](https://docs.aiinbx.com/reference/agents.md): Machine-readable docs for coding agents: llms.txt, Markdown copies of every page, the OpenAPI document, and the rules for implementing AI Inbx.

### SDKs

- [SDKs](https://docs.aiinbx.com/sdks.md): Official TypeScript and Python clients — same resources, same idempotency and webhook verification, idiomatic in each language.
- [TypeScript](https://docs.aiinbx.com/sdks/typescript.md): Install, configure, and use the aiinbx package — resources, pagination, response metadata, typed errors, React Email, and webhook verification.
- [Python](https://docs.aiinbx.com/sdks/python.md): Install, configure, and use the aiinbx package — sync and async clients, pagination, raw responses, typed errors, and webhook verification.

### Reference

- [Conventions](https://docs.aiinbx.com/reference/conventions.md): The rules that hold across every AI Inbx endpoint: prefixed IDs, cursor pagination, timestamps, request IDs, and idempotent retries.
- [Errors](https://docs.aiinbx.com/reference/errors.md): Every AI Inbx error code in the problem+json body, what each one means, how to recover from it, and which failures are worth retrying.
