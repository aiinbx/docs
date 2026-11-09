# SDKs

Official TypeScript and Python clients — same resources, same idempotency and webhook verification, idiomatic in each language.

[View on docs.aiinbx.com](https://docs.aiinbx.com/sdks)

Two official clients. They cover the same API v2 surface with the same resource names, and each is written the way its language expects rather than being a thin translation of the other.

**[TypeScript](typescript.md)**

Dependency-free. Node 20+, Bun, Deno, and edge runtimes.

**[Python](python.md)**

Sync and async clients on httpx. Python 3.10+.

## Install

```bash npm
npm install aiinbx
```
```bash pip
pip install aiinbx
```
Both read `AI_INBX_API_KEY` from the environment, so the common case needs no configuration:

```ts TypeScript
import AIInbx from "aiinbx"

const aiinbx = new AIInbx()

const email = await aiinbx.emails.send({
  from: "Ada <ada@example.com>",
  to: "grace@example.com",
  subject: "Hello",
  text: "Sent with AI Inbx.",
})
```
```python Python
from aiinbx import AIInbx

with AIInbx() as client:
    email = client.emails.send(
        {
            "from_": "Ada <ada@example.com>",
            "to": ["grace@example.com"],
            "subject": "Hello",
            "text": "Sent with AI Inbx.",
        }
    )
```
## What both give you

|                          | Behaviour                                                                                                                                                                                                                                                       |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Retries**              | Twice by default, with bounded exponential backoff, honoring `Retry-After`: a 429 always; network errors, 408 and 5xx when a retry can't act twice — a `GET`, `PUT` or `DELETE`, or a send, reply or forward with an idempotency key. Never a 409 or other 4xx. |
| **Idempotency**          | A per-request key on sends, replies and forwards.                                                                                                                                                                                                               |
| **Pagination**           | Await a call for one page, or iterate for every page with your filters preserved.                                                                                                                                                                               |
| **Request IDs**          | Reachable without unwrapping the response.                                                                                                                                                                                                                      |
| **Typed errors**         | A subclass per status, carrying `status`, `code`, `request_id`, headers, and the parsed body.                                                                                                                                                                   |
| **Webhook verification** | Timing-safe HMAC checking with a replay window, and a discriminated event union.                                                                                                                                                                                |
| **Types**                | Full TypeScript types; `py.typed` and typed dicts for Python.                                                                                                                                                                                                   |

## Resources

Resource names match across both clients, in each language's casing — `webhookEndpoints` and `webhook_endpoints`, `pacingRules` and `pacing_rules`. Every endpoint page in the [API reference](../api/README.md) shows the call in TypeScript, Python, and curl.

## Naming

Two differences to know before you copy a snippet between them.

**`from` is a reserved word in Python**, so the Python client uses `from_`:

```ts TypeScript
{
  from: "Ada <ada@example.com>"
}
```
```python Python
{"from_": "Ada <ada@example.com>"}
```
**Everything else keeps the wire name.** Request and response fields are `snake_case` in both — `thread_id`, `scheduled_at`, `suppression_key` — because they're the API's names, not the language's. Only the client's own options follow each language's convention: `baseURL` / `base_url`, `maxRetries` / `max_retries`, `idempotencyKey` / `idempotency_key`.

## Without an SDK

The API is plain HTTP with bearer auth and JSON bodies. Curl examples run throughout the guides, and the [API reference](../api/README.md) shows every endpoint called through both SDKs and with curl.

Three things to get right if you're writing your own client:

1. **Send an Idempotency-Key on every send**

    Otherwise a retry after a timeout sends twice. See
    [Idempotency](../reference/conventions.md#idempotency).

2. **Verify webhook signatures over the raw body**

    Before parsing. See [Verifying](../webhooks/verifying.md#any-language).

3. **Retry 429 and 5xx with backoff, and nothing else**

    Handle `408` and network timeouts as uncertain outcomes; retry sends only
    with the same idempotency key and payload. Inspect other `4xx` errors before
    retrying. See [Errors](../reference/errors.md#retry-or-not).
