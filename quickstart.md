# Quickstart

Get started with AI Inbx in a sandbox: get an API key, send your first email with the SDK, and answer the reply through a verified webhook.

[View on docs.aiinbx.com](https://docs.aiinbx.com/quickstart)

This guide sends an email, receives the reply and answers it in the same thread, all in a [sandbox](guides/sandbox.md): a workspace whose mail stays inside AI Inbx. It needs no plan, no DNS and no inbox of yours. When it works, [go live](guides/going-live.md) and the same code sends real mail.

To use an existing Gmail or Outlook account, see [Mailboxes](guides/mailboxes.md); mailboxes connect once the workspace is live. To have a coding agent do the setup in your project, hand it the [agent skills](skills.md).

## 1. Get an API key

Either sign up at [aiinbx.com](https://aiinbx.com/signup), where your first workspace starts as a sandbox, and create a `full` key under **API keys** in the console, or create a sandbox with no account:

```bash
curl https://api.aiinbx.com/api/v2/sandboxes \
  -H "Content-Type: application/json" \
  -d '{"name":"Yourapp"}'
```
The response carries the workspace, its provided domain and a `full` key in `api_key.key`. Configuring webhooks and domains requires `full`; a production sending worker can use a separate `sending` key.

Store the key in your server environment. It is shown only once. Do not commit it or expose it in browser code.

```bash
export AI_INBX_API_KEY="YOUR_API_KEY"
```
These examples read from the process environment. If you use a `.env` file, configure your runtime to load it before starting the application.

## 2. Install and initialize a client

Choose TypeScript, Python, or curl. The following steps reuse the initialized client.

```bash TypeScript
npm install aiinbx
```
```bash Python
pip install aiinbx
```
```ts TypeScript
import AIInbx from "aiinbx"

const aiinbx = new AIInbx()
```
```python Python
from aiinbx import AIInbx

client = AIInbx()
# Reuse this client for the steps below; call client.close() when finished.
```
## 3. Find your sender

Every workspace has a [provided domain](guides/domains.md#your-provided-domain), `<slug>.aiinbx.app`, that sends and receives with no DNS. Every address on it works.

```ts TypeScript
const domains = await aiinbx.domains.list().all()
const provided = domains.find((domain) => domain.provided)
console.log(provided?.name)
```
```python Python
provided = next(domain for domain in client.domains.iter() if domain["provided"])
print(provided["name"])
```
```bash curl
curl https://api.aiinbx.com/api/v2/domains \
  -H "Authorization: Bearer $AI_INBX_API_KEY"
```
The examples below send from `hello@yourapp.aiinbx.app`; use your own provided domain's name. A domain of your own works too: in a sandbox it is verified as you add it, and you publish its DNS records when you go live. See [Domains in a sandbox](guides/sandbox.md#domains).

## 4. Receive events

Set up the webhook handler before sending, so it is there when the reply arrives. This minimal handler uses the Web `Request` and `Response` APIs. Mount it in your app; see [Next.js](integrations/nextjs.md), [Hono](integrations/hono.md), or [Python verification](webhooks/verifying.md#python) for framework integration.

```ts
import { verifyWebhookRequest, WebhookSignatureError } from "aiinbx/webhooks"

export async function POST(request: Request) {
  let event
  try {
    event = await verifyWebhookRequest(
      request,
      process.env.AI_INBX_WEBHOOK_SECRET!
    )
  } catch (error) {
    if (error instanceof WebhookSignatureError) {
      return new Response("Invalid signature", { status: 400 })
    }
    throw error
  }

  // Development only: confirm that a verified event reaches your handler.
  console.log(event.id, event.type)
  if (event.type === "email.received") {
    console.log(event.data.email_id, event.data.thread_id)
  }
  return new Response(null, { status: 204 })
}
```
For local development you need neither a public endpoint nor a tunnel. [`aiinbx listen`](cli.md#listen-webhooks-on-localhost) forwards the workspace's events to `localhost`, signed with a secret of its own. It acts with `AI_INBX_API_KEY` from the environment:

```bash
export AI_INBX_WEBHOOK_SECRET=$(aiinbx listen --print-secret)
# start your app with that variable, then:
aiinbx listen --forward-to localhost:3000/api/webhooks
```
On a public server, create an endpoint under **Webhooks** in the console or with `webhookEndpoints.create`, subscribe it to `email.received`, `email.delivered` and `email.bounced`, and store its signing secret as `AI_INBX_WEBHOOK_SECRET`.

This handler only logs events. Before using events to perform work in production, [store them durably before acknowledging delivery](guides/production.md#receive-events-durably).

## 5. Send an email

Send to `reply@simulator.aiinbx.com`. In a sandbox it is reported delivered and then answers. Use one idempotency key for this send; use a new key when you intentionally send a different message.

```ts TypeScript
const email = await aiinbx.emails.send(
  {
    from: "hello@yourapp.aiinbx.app",
    to: "reply@simulator.aiinbx.com",
    subject: "Your first AI Inbx email",
    text: "Reply to this message to test receiving.",
  },
  { idempotencyKey: "quickstart-message-1" }
)
console.log(email.id, email.thread_id, email.status)
```
```python Python
email = client.emails.send(
    {
        "from_": "hello@yourapp.aiinbx.app",
        "to": ["reply@simulator.aiinbx.com"],
        "subject": "Your first AI Inbx email",
        "text": "Reply to this message to test receiving.",
    },
    idempotency_key="quickstart-message-1",
)
print(email["id"], email["thread_id"], email["status"])
```
```bash curl
curl https://api.aiinbx.com/api/v2/emails \
  -H "Authorization: Bearer $AI_INBX_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: quickstart-message-1" \
  -d '{
    "from":"hello@yourapp.aiinbx.app",
    "to":"reply@simulator.aiinbx.com",
    "subject":"Your first AI Inbx email",
    "text":"Reply to this message to test receiving."
  }'
```
The response contains an email `id`, a `thread_id`, a send `status`, and `sandbox: true`. Save both IDs. Inspect `suppressed` for excluded recipients and `pacing` for a hold; see [Reading the response](guides/sending.md#reading-the-response).

## 6. Receive the reply

A few seconds later your handler logs `email.delivered`, and then `email.received` for the simulator's answer, with the same `thread_id` as your send. If nothing arrives, check that `listen` is running or that your endpoint is subscribed to `email.received`, and look at the email's `events` with `emails.retrieve`.

To test other outcomes, send to `bounce@simulator.aiinbx.com` or `complaint@simulator.aiinbx.com`. To test mail that starts a conversation, write an email into the sandbox with [`sandboxEmails.receive`](guides/sandbox.md#write-inbound-mail).

## 7. Continue the conversation

Use the `thread_id` and `email_id` from the received event. The API infers the reply's addressing and headers.

```ts TypeScript
await aiinbx.threads.reply(
  "YOUR_THREAD_ID",
  {
    email_id: "YOUR_EMAIL_ID",
    text: "Your reply arrived. The integration is working.",
  },
  { idempotencyKey: "reply-YOUR_EMAIL_ID" }
)
```
```python Python
client.threads.reply(
    "YOUR_THREAD_ID",
    {"email_id": "YOUR_EMAIL_ID", "text": "Your reply arrived. The integration is working."},
    idempotency_key="reply-YOUR_EMAIL_ID",
)
client.close()
```
```bash curl
curl https://api.aiinbx.com/api/v2/threads/YOUR_THREAD_ID/reply \
  -H "Authorization: Bearer $AI_INBX_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: reply-YOUR_EMAIL_ID" \
  -d '{"email_id":"YOUR_EMAIL_ID","text":"Your reply arrived. The integration is working."}'
```
The thread now holds three emails: yours, the simulator's answer, and your reply. The simulator answers an email once, so the round trip ends here. To test a longer conversation, answer your reply with [`sandboxEmails.reply`](guides/sandbox.md#answer-a-sent-email).

## 8. Go live

When the integration works, the workspace's owner chooses a plan and [goes live](guides/going-live.md), with **Go live** in the console or through the API. A sandbox created through the API first needs an owner: create a [claim link](guides/going-live.md#claim-a-sandbox) and give it to the person who will own the workspace. After the claim, the sandbox's keys are the owner's and keep working, going live included once there is a plan. Going live keeps your keys, webhook endpoints and domains by default, and clears the test mail. Then publish the DNS records of any domain of your own, and send one email to an address you control to check delivery for real.

Use the [production guide](guides/production.md) to add durable event processing, retries, tenant authorization, and operational monitoring. The [API reference](api/README.md) lists the full request and response schemas.

To let an AI agent handle the conversation, continue with [Build an AI email agent](integrations/agents.md): retrieve thread context, persist a reply for review, and send it with a stable idempotency key.
