# AI Inbx documentation

Learn to send and receive email, manage conversation threads, and build AI email agents with the AI Inbx API, TypeScript SDK, and Python SDK.

[View on docs.aiinbx.com](https://docs.aiinbx.com)

AI Inbx provides APIs for sending email, receiving messages, and managing conversations. Use it to build transactional email, customer communication, or email agents with verified domains and connected Gmail or Outlook accounts.

Each email belongs to a [thread](guides/threads.md). Reply with a thread ID and message content; the API derives the sender, recipients, subject, and reply headers. [Signed webhooks](webhooks/README.md) notify your application about incoming messages and delivery outcomes.

**[Quickstart](quickstart.md)**

Create a key, configure a sender, and send your first email.

**[API reference](api/README.md)**

Every endpoint, generated from the schemas the API validates against.

**[TypeScript SDK](sdks/typescript.md)**

Dependency-free, typed, works on Node, Bun, Deno, and edge runtimes.

**[Python SDK](sdks/python.md)**

Sync and async clients with the same resource surface.

Read [Core concepts](concepts.md) for the resource model, or choose a framework from [Integrations](integrations/README.md).

## What it does

**Send**

HTML or text, attachments, custom headers, idempotent retries, and sends
scheduled up to 30 days out.

**Receive**

Inbound mail on any domain you can publish DNS for, or through a
connected Gmail or Outlook mailbox, delivered as webhooks.

**Thread**

Replies land on the conversation they belong to, even when headers are
missing or the subject changed. A shared subject alone never merges two.

**Find conversations**

List threads by subject or participant, filter by mailbox or space,
and paginate through their messages.

**Suppress**

Bounces, complaints, and unsubscribes stop the next send automatically,
per list or across the org.

**Pace**

Sending windows, rate ceilings, and a queue you can inspect and release
by hand.

## Choose your starting point

| You want to | Start here |
| --- | --- |
| Send from a domain you control | [Quickstart](quickstart.md) |
| Connect an existing Gmail or Outlook account | [Mailboxes](guides/mailboxes.md) |
| Provide email for multiple customers | [Spaces](guides/spaces.md) |
| Build an automated email workflow | [Email agents](integrations/agents.md) |
| Prepare an integration for production | [Production guide](guides/production.md) |
| Give a coding agent the API contract | [Documentation for agents](reference/agents.md) |
| Work in AI Inbx from Claude, ChatGPT or Cursor | [MCP server](mcp/README.md) |
| Give an agent an email address of its own | [Agent inbox](guides/agent-inbox.md) |
| Run AI Inbx from a terminal or script | [CLI](cli.md) |

## Send an email

Install an [SDK](sdks/README.md), set `AI_INBX_API_KEY`, and configure a [verified sending domain](guides/domains.md) or [connected mailbox](guides/mailboxes.md). Replace the example addresses with your sender and a recipient you control.

```ts TypeScript
import AIInbx from "aiinbx"

const aiinbx = new AIInbx() // reads AI_INBX_API_KEY

const email = await aiinbx.emails.send({
  from: { name: "Ada", address: "ada@example.com" },
  to: "grace@example.com",
  subject: "Quick question",
  text: "Does Thursday still work for the review?",
})

console.log(email.id, email.thread_id)
```
```python Python
from aiinbx import AIInbx

with AIInbx() as client:  # reads AI_INBX_API_KEY
    email = client.emails.send(
        {
            "from_": {"name": "Ada", "address": "ada@example.com"},
            "to": ["grace@example.com"],
            "subject": "Quick question",
            "text": "Does Thursday still work for the review?",
        }
    )

print(email["id"], email["thread_id"])
```
```bash curl
curl https://api.aiinbx.com/api/v2/emails \
  -H "Authorization: Bearer $AI_INBX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "from": "Ada <ada@example.com>",
    "to": "grace@example.com",
    "subject": "Quick question",
    "text": "Does Thursday still work for the review?"
  }'
```
When Grace replies, an [`email.received`](webhooks/events/email-received.md) webhook arrives carrying the same `thread_id`, and [`threads.reply`](guides/threads.md#replying) continues the conversation without you rebuilding a single header.

## Where to go next

**[Sending](guides/sending.md)**

Addresses, content, attachments, and idempotency.

**[Receiving](guides/receiving.md)**

Domains and mailboxes as two ways in.

**[Webhooks](webhooks/README.md)**

Endpoints, routing, retries, and signature verification.

**[Conventions](reference/conventions.md)**

IDs, pagination, request IDs, and idempotency in one place.
