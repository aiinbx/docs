# Sandbox

Build and test AI Inbx in a sandbox workspace: no account or plan, simulated delivery and inbound mail, real webhooks, and its limits.

[View on docs.aiinbx.com](https://docs.aiinbx.com/guides/sandbox)

A sandbox is a workspace whose mail stays inside AI Inbx. It has the whole API: the same keys, domains, emails, threads, events and webhooks as a live workspace. Nothing it sends is delivered, nothing from outside arrives in it, and it needs no plan and is billed nothing. When the integration works, a person [goes live](going-live.md) and the same keys send real mail.

Every new workspace starts as a sandbox, whether a person signed up for it or an agent created it through the API. `GET /workspace` says which a workspace is: `sandbox` is `true` until it goes live.

## Get a sandbox

There are four ways to get one.

- **Sign up** at [aiinbx.com](https://aiinbx.com/signup). Your first workspace is a sandbox. Create an API key under **API keys** in the console.
- **Create one through the API**, with no account and no credential. This is the way for a coding agent that has nobody signed in.
- **Create one with the [CLI](../cli.md#sandbox)**: `aiinbx sandboxes create --name "Yourapp"` needs no login either, and signs the CLI in with the new key.
- **Add one to your account** for a new project, [below](#a-sandbox-on-your-account).

```bash curl
curl https://api.aiinbx.com/api/v2/sandboxes \
  -H "Content-Type: application/json" \
  -d '{"name":"Yourapp"}'
```
```ts TypeScript
import AIInbx from "aiinbx"

// A client without a key can make this one call.
const created = await new AIInbx().sandboxes.create({ name: "Yourapp" })
console.log(created.workspace.slug, created.domain?.name)
// created.api_key.key is the secret: store it, do not log it.
```
```python Python
from aiinbx import AIInbx

with AIInbx() as client:  # no key needed for this call
    created = client.sandboxes.create({"name": "Yourapp"})
print(created["workspace"]["slug"], created["domain"] and created["domain"]["name"])
# created["api_key"]["key"] is the secret: store it, do not log it.
```
The response is `201` with three fields:

| Prop | Type | Default | Description |
| - | - | - | - |
| `workspace` | `object` | - | The new workspace, with `sandbox: true`. Its `slug` follows `name`; without a name, one is made up. |
| `domain` | `object \| null` | - | Its provided domain, `<slug>.aiinbx.app`, ready to send from. Null when it could not be made at once; it is made shortly after. |
| `api_key` | `object` | - | A `full` key for the workspace. The secret is in `api_key.key`, returned this once. |

A workspace created this way has no members. Its keys can do everything an admin can, such as creating more API keys and changing the workspace's settings, except inviting people and going live: those need an owner, so `POST /invitations` and `POST /workspace/go-live` answer `403` [`forbidden`](../reference/errors.md#forbidden) until a person has claimed the sandbox. To hand it to a person, [create a claim link](going-live.md#claim-a-sandbox). At the claim, the sandbox's keys become keys of the person who claimed it, an owner, and keep doing everything they did before. From then on they can also invite people, and go live once the workspace has a plan. Like any key, each then acts with its creator's role in the workspace, so whoever holds them can use the workspace after the claim. A sandbox nobody has claimed is deleted, with everything in it, 14 days after its last API call. One a person has claimed is never deleted this way.

Each IP address can create 10 sandboxes an hour, an IPv6 /64 counting as one address. More fail with `429` [`too_many_requests`](../reference/errors.md#too_many_requests) and a `Retry-After` header. When the limit cannot be checked, the request fails with `503` [`unavailable`](../reference/errors.md#unavailable) and a `Retry-After` header.

### A sandbox on your account

Signed in, a new project gets a workspace of its own: **New workspace** in the console, [`POST /workspaces`](../api/endpoints/workspace/create-workspace.md) through the API, `aiinbx_workspaces_create` on the [MCP server](../mcp/tools.md), or `aiinbx workspaces create --name "Yourapp" --env-file .env.local` with the CLI. You own it from the start, so there is nothing to claim. The response has the same three fields as above; its key is yours, with `full` scope, and goes into the app's env file. An OAuth sign-in can act in it at once: the MCP server with its `workspace_id`, the CLI after `aiinbx workspace use <slug>`. A connection made with an API key stays in that key's workspace; the app uses the new key. A key of a sandbox nobody has claimed acts for nobody, so it cannot create one: `403` [`forbidden`](../reference/errors.md#forbidden).

## What happens to mail

Sending works as it does in a live workspace, up to the point where mail would leave. A send is validated, checked against suppression lists and [pacing rules](pacing.md), stored, and answered with the email and its thread. It is never handed to a mail provider. About two seconds later, AI Inbx reports on it the way a provider would: `email.sent`, then `email.delivered`, `email.bounced` or `email.complained` for each recipient, and the email's `status` follows.

What each recipient gets:

| Recipient                        | What happens                                                                                                                                                                             |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `reply@simulator.aiinbx.com`     | Delivered, then answered. The answer arrives in the same thread as `email.received`, from `reply@simulator.aiinbx.com`, quoting the email and with the reply headers a mail client sets. |
| `bounce@simulator.aiinbx.com`    | Bounces as a permanent failure (`550 5.1.1 user unknown`): `email.bounced`, and the address is [suppressed](suppressions.md) as a dead one.                                         |
| `complaint@simulator.aiinbx.com` | Delivered, then marked as spam: `email.delivered`, then `email.complained`, and the address is suppressed.                                                                               |
| An address of the workspace      | Delivered, and arrives there as inbound mail with its own `email.received`, as mail between two of your addresses does in a live workspace.                                              |
| Any other address                | Reported delivered. Nothing is sent to it.                                                                                                                                               |

Only the simulator's addresses ever bounce or complain, so testing in a sandbox never suppresses a real address. After a bounce or a complaint the simulator address is on the suppression list like any other, and the next send to it comes back with it in `suppressed` (or fails with `400` `recipients_suppressed` when it leaves no `to`). Delete the suppression to test the same outcome again.

`reply@simulator.aiinbx.com` answers an email once. Your app's answer to its reply gets no answer, so a round trip of send, reply and your app's answer ends there. To continue the conversation, write the next reply with [`POST /sandbox/emails/{email_id}/reply`](#answer-a-sent-email).

## Write inbound mail

Mail from outside never reaches a sandbox. Write it in through the API instead: `POST /sandbox/emails` composes an email from what you pass and puts it through everything real inbound mail goes through, including threading, `stripped_text`, categories, attachment preparation and the `email.received` webhook.

```ts TypeScript
const email = await aiinbx.sandboxEmails.receive({
  from: "Dana <dana@example.net>",
  to: "support@yourapp.aiinbx.app",
  subject: "Order 1042 never arrived",
  text: "Hi, my order was due on Monday. Can you check where it is?",
})
console.log(email.id, email.thread_id)
```
```python Python
email = client.sandbox_emails.receive(
    {
        "from_": "Dana <dana@example.net>",
        "to": ["support@yourapp.aiinbx.app"],
        "subject": "Order 1042 never arrived",
        "text": "Hi, my order was due on Monday. Can you check where it is?",
    }
)
print(email["id"], email["thread_id"])
```
```bash curl
curl https://api.aiinbx.com/api/v2/sandbox/emails \
  -H "Authorization: Bearer $AI_INBX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "from": "Dana <dana@example.net>",
    "to": "support@yourapp.aiinbx.app",
    "subject": "Order 1042 never arrived",
    "text": "Hi, my order was due on Monday. Can you check where it is?"
  }'
```
`from` can be any address, so mail from any sender can be simulated. Every `to` has to be an address on one of the workspace's domains, or the request fails with `422` [`recipient_unknown`](../reference/errors.md#recipient_unknown). `cc`, `reply_to`, `html` and up to 20 `attachments` are accepted as on a send. To write mail into an existing conversation, pass the Message-ID it answers as `in_reply_to`, and the conversation's Message-IDs as `references`.

The response is `201` with the email filed for the first `to`. As with real mail, a copy is filed in each [space](spaces.md) the recipients are in. A bounce report written in is handled as a real one is and not filed as an email, and the request fails with `400` `invalid_request`; to try a bounce, send to `bounce@simulator.aiinbx.com`. The call needs a `sending` or `full` key.

[Attachments](attachments.md) are prepared as in a live workspace, except scanned PDF pages: a sandbox skips them, whether or not it has a plan, so such an attachment is `partial`, or `failed` when every page is scanned, and its `warnings` count the pages skipped.

### Headers and verdicts

`headers` adds headers to the simulated email, as an object of header name to value. This request simulates an out-of-office reply, which carries `Auto-Submitted: auto-replied`:

```ts TypeScript
const autoReply = await aiinbx.sandboxEmails.receive({
  from: "Dana <dana@example.net>",
  to: "support@yourapp.aiinbx.app",
  subject: "Out of office: Order 1042 never arrived",
  text: "I am out of the office until Monday and will answer then.",
  headers: { "Auto-Submitted": "auto-replied" },
})
```
```python Python
auto_reply = client.sandbox_emails.receive(
    {
        "from_": "Dana <dana@example.net>",
        "to": ["support@yourapp.aiinbx.app"],
        "subject": "Out of office: Order 1042 never arrived",
        "text": "I am out of the office until Monday and will answer then.",
        "headers": {"Auto-Submitted": "auto-replied"},
    }
)
```
```bash curl
curl https://api.aiinbx.com/api/v2/sandbox/emails \
  -H "Authorization: Bearer $AI_INBX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "from": "Dana <dana@example.net>",
    "to": "support@yourapp.aiinbx.app",
    "subject": "Out of office: Order 1042 never arrived",
    "text": "I am out of the office until Monday and will answer then.",
    "headers": { "Auto-Submitted": "auto-replied" }
  }'
```
A `List-Unsubscribe` header simulates mail from a mailing list the same way.

`verdicts` sets the results of the receiving checks: `spam`, `spf`, `dkim` and `dmarc`, each `PASS` or `FAIL`, and `PASS` for any you leave out. They show on the email's `verdicts`, and an email whose `spam` check failed arrives with the category `spam`.

## Answer a sent email

`POST /sandbox/emails/{email_id}/reply` answers an email the sandbox sent, as one of its recipients would: to its Reply-To, else its sender, with `Re:` before the subject, the original quoted below, and `In-Reply-To` and `References` naming it. The answer arrives like any inbound reply: in the email's thread when it lands in the same space.

```ts TypeScript
const reply = await aiinbx.sandboxEmails.reply(email.id, {
  text: "Thanks, that solves it.",
})
```
```python Python
reply = client.sandbox_emails.reply(email["id"], {"text": "Thanks, that solves it."})
```
```bash curl
curl https://api.aiinbx.com/api/v2/sandbox/emails/eml_.../reply \
  -H "Authorization: Bearer $AI_INBX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"text":"Thanks, that solves it."}'
```
`from` defaults to the email's first `to`; pass another address to answer as someone else on it. Without `text` or `html`, the answer is a short one AI Inbx writes. An email that has not gone out, or one the workspace received, fails with `409` [`not_sent`](../reference/errors.md#not_sent). An email whose Reply-To names nobody of the workspace fails with `422` `recipient_unknown`, since the answer would go nowhere AI Inbx files mail.

Use this to answer from an address that is not the simulator's, to write a specific answer, or to continue a conversation after the simulator's one answer: each of your app's answers can be answered this way in turn. `reply@simulator.aiinbx.com` covers a single round trip without a call.

## Webhooks

Webhook endpoints are real. Events are delivered to your URLs, signed, retried and recorded exactly as in a live workspace, so a sandbox tests your handler, its signature check and its queue. An endpoint's URL has to be public HTTPS here too. For a server on your machine, [`aiinbx listen`](../cli.md#listen-webhooks-on-localhost) forwards the same signed events to `localhost`.

## Domains

A domain added to a sandbox is verified as it is added, and `domain.verified` fires. There is no DNS to publish yet: the `records` it returns are the ones you publish once the workspace goes live, when the domain goes back to pending and is verified through DNS like any new domain. See [Going live](going-live.md#what-happens-to-domains).

A sandbox's domain never takes the name from anyone. Another workspace can have the same name, and mail to it from outside never reaches the sandbox. Wildcards, subdomains, [spaces](spaces.md) and [assigned addresses](spaces.md#single-addresses-of-one-domain) work as they do in a live workspace.

The [provided domain](domains.md#your-provided-domain) works in a sandbox too, with its [ceilings](pacing.md#the-provided-domains-ceilings). Pacing holds mail over them until they allow it, so the eleventh email of a day from it to `reply@simulator.aiinbx.com` goes the next day. Add a domain of your own to the sandbox to test more than that.

## What needs a live workspace

These answer `403` [`sandbox_unavailable`](../reference/errors.md#sandbox_unavailable) in a sandbox:

- Connecting a Gmail or Outlook [mailbox](mailboxes.md), and registering an [OAuth app](mailboxes.md#your-own-oauth-app) for it.
- A domain's delivery test, which reads its DNS records and signing.

A sandbox nobody has claimed has no members, so it cannot invite anyone or go live: `POST /invitations` and `POST /workspace/go-live` answer `403` [`forbidden`](../reference/errors.md#forbidden). Create a claim link for the person who should own it instead.

## Limits

| Limit         | Value                                           | At the limit                                                             |
| ------------- | ----------------------------------------------- | ------------------------------------------------------------------------ |
| Emails        | 2,000 a day (UTC), 500 until a person claims it | `429` [`sandbox_limit_reached`](../reference/errors.md#sandbox_limit_reached) |
| New sandboxes | 10 an hour per IP address                       | `429` [`too_many_requests`](../reference/errors.md#too_many_requests)         |

Emails are counted as a plan counts them, sent and received. Each of these counts as one of the day's emails:

- each send, when AI Inbx takes it, whatever its recipients, including a scheduled send or one held by pacing; it is not given back if it is canceled or fails
- each copy filed at an address of the workspace, one per space it is filed in: a send's copy at an address of its own, the answer from `reply@simulator.aiinbx.com`, and mail written in with [`POST /sandbox/emails`](#write-inbound-mail) or [`POST /sandbox/emails/{email_id}/reply`](#answer-a-sent-email)
- each [webhook test](../webhooks/README.md#testing-an-endpoint)

So a send to `reply@simulator.aiinbx.com` counts twice: the send and its answer. Once the day's emails are used up, sends fail with `sandbox_limit_reached`. A send that takes the last one is still reported delivered, and files no copy at the workspace's own addresses and gets no answer from the simulator. Sandboxes nobody has claimed also share a ceiling of 1,000,000 emails a day (UTC) between all of them; should they reach it, they get `sandbox_limit_reached` too.

What a sandbox holds has ceilings. A request past one fails with `403` [`sandbox_unavailable`](../reference/errors.md#sandbox_unavailable), and its `detail` names the limit. Delete one to make room.

| Ceiling                            | Value |
| ---------------------------------- | ----- |
| Custom domains                     | 10    |
| Subdomains under its own wildcards | 100   |
| Spaces                             | 25    |
| Addresses filed in a space         | 100   |
| API keys                           | 20    |
| Webhook endpoints                  | 5     |
| Custom pacing rules, on or off     | 10    |
| Suppressions added by hand         | 1,000 |
| Drafts                             | 100   |
| Mailboxes                          | None  |

A person can own at most 20 sandbox workspaces; live workspaces do not count. Creating a twenty-first in the console, or claiming one with a claim link, is refused until one of the 20 goes live or is deleted.

Going live lifts all of these and puts the workspace on its plan's allowances. The [request budget](../reference/conventions.md#request-budgets) applies as in a live workspace.

## Telling sandbox data apart

Every email, every send, reply and forward response, and every webhook envelope carries `sandbox`: `true` for anything that happened in a sandbox. When one handler serves a sandbox and a live workspace, branch on it before doing anything outside your application, such as charging a customer or paging someone.

```ts
if (event.sandbox) {
  // A test: record it, but do not act on it outside the app.
}
```
Going live removes everything the sandbox produced, so a live workspace holds no email with `sandbox: true`.

## Related

- [Claim and go live](going-live.md): handing a sandbox to a person, and making it live.
- [Quickstart](../quickstart.md): a sandbox, one send and its reply, step by step.
- [Errors](../reference/errors.md#sandbox): every code above.
