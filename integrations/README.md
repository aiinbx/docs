# Integrate AI Inbx

Integrate AI Inbx with your stack: Next.js and Hono webhook handlers, React Email templates, and agent loops that send, read and reply to mail.

[View on docs.aiinbx.com](https://docs.aiinbx.com/integrations)

The [guides](../guides/sending.md) cover what each endpoint does. These pages cover the wiring around them — the parts that are the same for everyone and annoying to get right the first time.

There are only two integration points. Sending is an outbound HTTP call you can make from anywhere. Receiving is an inbound HTTP request you have to authenticate, and that's where the framework-specific detail lives.

**[Next.js](nextjs.md)**

A route handler for webhooks, and sending from a server action.

**[Hono](hono.md)**

The same handler on Workers, Bun, Deno, and Node.

**[React Email](react-email.md)**

Pass a component as `react` and skip the render step.

**[Email agents](agents.md)**

The reply loop, and the three ways it goes wrong.

## Anywhere else

The TypeScript SDK is dependency-free and uses only `fetch` and Web Crypto, so it runs unmodified on Node 20+, Bun, Deno, Cloudflare Workers, Vercel Edge, and Netlify. Nothing on these pages needs a Node-only API.

If your framework isn't listed, the shape is always the same:

```ts
import { verifyWebhookRequest } from "aiinbx/webhooks"

// Give the helper the whole Request — it needs the raw body, and any
// framework that has parsed the JSON for you has already destroyed it.
const event = await verifyWebhookRequest(request, process.env.AI_INBX_WEBHOOK_SECRET!)
```
**[Verifying requests](../webhooks/verifying.md)**

The signature scheme itself, and verifying without the SDK.
