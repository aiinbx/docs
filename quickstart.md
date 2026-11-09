# Quickstart

Get started with AI Inbx: add a sending domain, send your first email with the SDK, and receive the reply through a verified webhook.

[View on docs.aiinbx.com](https://docs.aiinbx.com/quickstart)

This guide sends an email from your domain and receives a reply. You need an AI Inbx workspace, access to your domain's DNS, and a recipient address you control. DNS verification can take time; there is no fixed completion time for this step.

To use an existing Gmail or Outlook account without changing DNS, start with [Mailboxes](guides/mailboxes.md). To have a coding agent do the setup in your project, hand it the [agent skills](skills.md).

## 1. Create an API key

Open **API keys** in the [console](https://aiinbx.com/app) and create a `full` key for this setup. Configuring domains and webhooks requires `full`; a production sending worker can use a separate `sending` key.

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
## 3. Configure your sending domain

Replace `mail.example.com` with a subdomain you control. A dedicated subdomain lets you configure inbound mail without changing the routing of your existing business inboxes.

```ts TypeScript
const domain = await aiinbx.domains.create({ name: "mail.example.com" })
console.log(domain.id, domain.records)
```
```python Python
domain = client.domains.create({"name": "mail.example.com"})
print(domain["id"], domain["records"])
```
```bash curl
curl https://api.aiinbx.com/api/v2/domains \
  -H "Authorization: Bearer $AI_INBX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"name":"mail.example.com"}'
```
Publish the DNS records returned by this call, using their exact names, types, and values. DKIM verification enables sending. Publish the `INBOUND` MX record too if you want replies to arrive through AI Inbx. Review existing DNS records before changing them; an inbound MX change affects where that domain receives mail.

Then request verification:

```ts TypeScript
const verified = await aiinbx.domains.verify(domain.id)
console.log(verified.verified_at)
```
```python Python
verified = client.domains.verify(domain["id"])
print(verified["verified_at"])
```
```bash curl
curl -X POST https://api.aiinbx.com/api/v2/domains/YOUR_DOMAIN_ID/verify \
  -H "Authorization: Bearer $AI_INBX_API_KEY"
```
Continue when `verified_at` is non-null. If it remains null, use [domain diagnostics](guides/domains.md#diagnostics). Sending verification alone does not confirm that your inbound MX is configured.

## 4. Send an email

Replace the sender with an address on your verified domain and the recipient with an inbox you control. Use one idempotency key for this test; use a new key when you intentionally send a different message.

```ts TypeScript
const email = await aiinbx.emails.send(
  {
    from: "hello@mail.example.com",
    to: "you@example.net",
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
        "from_": "hello@mail.example.com",
        "to": ["you@example.net"],
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
    "from":"hello@mail.example.com",
    "to":"you@example.net",
    "subject":"Your first AI Inbx email",
    "text":"Reply to this message to test receiving."
  }'
```
The response contains an email `id`, a `thread_id`, and a send `status`. Save both IDs. A successful API response does not guarantee inbox delivery. Inspect `suppressed` for excluded recipients and `pacing` for a hold; see [Reading the response](guides/sending.md#reading-the-response).

## 5. Receive a reply

Create a public HTTPS webhook endpoint in **Webhooks** in the console. Subscribe to `email.received`, `email.delivered`, and `email.bounced`, and store the signing secret as `AI_INBX_WEBHOOK_SECRET` in your server environment. For local development you need neither the endpoint nor a tunnel: [`aiinbx listen`](cli.md#listen-webhooks-on-localhost) forwards the same signed events to `localhost`.

This minimal handler uses the Web `Request` and `Response` APIs. Mount it at the URL you registered; see [Next.js](integrations/nextjs.md), [Hono](integrations/hono.md), or [Python verification](webhooks/verifying.md#python) for framework integration.

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
Reply from the recipient inbox. Your handler should receive `email.received` with the conversation's `thread_id`. If it does not, check the inbound MX record, webhook subscriptions, and delivery attempts in the console.

This handler only logs events. Before using events to perform work in production, [store them durably before acknowledging delivery](guides/production.md#receive-events-durably).

## 6. Continue the conversation

Use the `thread_id` from the received event. Replace the placeholders below with the actual thread and email IDs. The API infers the reply's addressing and headers.

```ts TypeScript
await aiinbx.threads.reply(
  "YOUR_THREAD_ID",
  { text: "Your reply arrived. The integration is working." },
  { idempotencyKey: "reply-YOUR_EMAIL_ID" }
)
```
```python Python
client.threads.reply(
    "YOUR_THREAD_ID",
    {"text": "Your reply arrived. The integration is working."},
    idempotency_key="reply-YOUR_EMAIL_ID",
)
client.close()
```
```bash curl
curl https://api.aiinbx.com/api/v2/threads/YOUR_THREAD_ID/reply \
  -H "Authorization: Bearer $AI_INBX_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: reply-YOUR_EMAIL_ID" \
  -d '{"text":"Your reply arrived. The integration is working."}'
```
You now have a send-and-reply flow. Use the [production guide](guides/production.md) to add durable event processing, retries, tenant authorization, and operational monitoring. The [API reference](api/README.md) lists the full request and response schemas.

To let an AI agent handle the conversation, continue with [Build an AI email agent](integrations/agents.md): retrieve thread context, persist a reply for review, and send it with a stable idempotency key.
