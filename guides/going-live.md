# Claim and go live

Hand a sandbox to a person with a claim link, then go live: the plan it needs, what is cleared, what keep keeps, and what happens to domains.

[View on docs.aiinbx.com](https://docs.aiinbx.com/guides/going-live)

A [sandbox](sandbox.md) becomes a live workspace in two separate steps. Claiming gives a sandbox an agent created an owner. Going live makes its mail real. A sandbox a person signed up for already has its owner and only needs the second step.

## Claim a sandbox

A sandbox created with `POST /sandboxes` has no members. Create a claim link with one of its `full` keys and hand the URL to the person who should own it.

```ts TypeScript
const link = await aiinbx.claimLinks.create()
console.log(link.url, link.expires_at)
```
```python Python
link = client.claim_links.create()
print(link["url"], link["expires_at"])
```
```bash curl
curl -X POST https://api.aiinbx.com/api/v2/claim-links \
  -H "Authorization: Bearer $AI_INBX_API_KEY"
```
With the CLI: `aiinbx claim-links create`.

AI Inbx does not send the link anywhere. Pass it on however suits you: in the chat with the person, in your own product, in an email of your own.

A link works once and for 7 days. Creating another replaces the one before it, so only the newest works. Once someone has claimed the workspace, creating a link fails with `409` [`already_claimed`](../reference/errors.md#already_claimed).

The person who opens the link sees the workspace's name, when it was created, and the API keys that will keep working, by their prefix. They sign up or log in, accept the terms, and become the workspace's owner. The console then opens on it. A person who already owns 20 sandbox workspaces cannot claim another: the page says so, and the claim works once one of theirs has gone live or been deleted.

The workspace stays a sandbox and everything in it stays. Its keys become keys of the new owner and keep doing everything they did before, including creating and revoking API keys and changing workspace settings. From the claim on they can also send invitations, and go live once the workspace has a plan. Like any key, each acts with its creator's role in the workspace, so whoever holds the sandbox's keys can use the workspace after the claim.

A claimed sandbox is no longer deleted after 14 idle days, has 2,000 emails a day instead of 500, and stops sharing the daily emails of sandboxes nobody has claimed.

## Go live

Going live makes the workspace's mail real: sends are delivered, mail from outside arrives, and both are billed to its plan.

It needs a [plan](billing.md#plans), a free one included, and an owner. Only the workspace's owner and admins can go live. A sandbox nobody has claimed cannot: `POST /workspace/go-live` answers `403` [`forbidden`](../reference/errors.md#forbidden) until a person has [claimed it](#claim-a-sandbox).

In the [console](https://aiinbx.com/app), a sandbox shows the line "Sandbox. Mail is simulated, nothing is delivered." with a **Go live** action on the Threads and Emails pages and under **Settings → Billing**. It opens a dialog that first offers the plans if the workspace has none, then one checkbox, **Keep what's already set up (API keys, webhooks, domains, settings)**, checked by default, and the **Go live** button. While the box is checked, the dialog also says how many API keys and webhook endpoints were created through the API before anyone owned the workspace, if there are any. The dialog shows when the workspace is live.

Through the API, call it with a key an admin created or a sign-in that acts as an admin. After a claim, that includes the keys the sandbox had, so the agent that built the integration can go live with its own key once the owner has chosen a plan. Without a plan it fails with `402` [`plan_required`](../reference/errors.md#plan_required).

```ts TypeScript
const workspace = await aiinbx.workspaces.goLive({ keep: true })
console.log(workspace.going_live) // true
```
```python Python
workspace = client.workspaces.go_live({"keep": True})
print(workspace["going_live"])  # True
```
```bash curl
curl https://api.aiinbx.com/api/v2/workspace/go-live \
  -H "Authorization: Bearer $AI_INBX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"keep":true}'
```
With the CLI: `aiinbx workspaces go-live`, or `aiinbx workspaces go-live --no-keep` to start empty.

A workspace goes live once. Calling it on a live workspace fails with `409` [`already_live`](../reference/errors.md#already_live).

Going live clears every email with its idempotency key, so a send your app retries afterwards is a new send, and a real one. Stop anything that retries test sends before going live.

### What is cleared

Everything the sandbox produced is removed, whatever `keep` says:

- emails, threads and attachments
- events and webhook deliveries
- suppressions, including the simulator addresses that bounced or complained
- pacing counters
- every [space](spaces.md), with everything in it: its domains (with every subdomain of its wildcards, wherever it is filed), assigned addresses, pacing rules and suppressions

### What `keep` keeps

`keep` is `true` unless you pass `false`.

|                                                                            | `keep: true`             | `keep: false`                           |
| -------------------------------------------------------------------------- | ------------------------ | --------------------------------------- |
| API keys                                                                   | Kept, and keep working   | Revoked, the one that made the call too |
| Webhook endpoints                                                          | Kept, with their secrets | Removed                                 |
| Domains outside spaces                                                     | Kept, back to pending    | Removed                                 |
| Pacing rules outside spaces                                                | Kept                     | Removed                                 |
| [Agent inboxes](agent-inbox.md) and assigned addresses outside spaces | Kept                     | Removed                                 |
| Workspace settings                                                         | Kept                     | Back to the defaults of a new workspace |
| Provided domain and its ceilings                                           | Kept                     | Kept                                    |

With `keep: true`, the integration you built in the sandbox runs on against the live workspace without changing a key or a webhook secret.

### What happens to domains

A kept domain goes back to pending: `verified_at` is `null` and its `records` are what to publish. Publish them at the domain's DNS host, as for a domain added to a live workspace, and AI Inbx verifies it from DNS; `domain.verified` fires when it does. Until then, sending from it fails with `409` [`domain_unverified`](../reference/errors.md#domain_unverified). A subdomain under a kept wildcard follows its wildcard. See [Domains](domains.md#the-records) for the records and [diagnostics](domains.md#diagnostics) for what is missing.

The [provided domain](domains.md#your-provided-domain) stays verified and sends at once.

### While it goes live

The call returns `202` with the workspace and `going_live: true`. Clearing the sandbox is usually done within minutes, and the workspace stays a sandbox until it is done. Meanwhile the API refuses every change with `409` [`going_live`](../reference/errors.md#going_live). Reads answer what the workspace keeps outside spaces, such as its domains, API keys and webhook endpoints, while its mail and everything in its spaces are being removed.

Read the workspace until `sandbox` is `false`. Going live with `keep: false` revokes every API key when it is done, the one reading included: then the read answers `401`, and the console or a signed-in CLI shows the result.

```ts TypeScript
const workspace = await aiinbx.workspaces.retrieve()
if (!workspace.sandbox) {
  // Live: publish DNS for kept domains, then send to a real address.
}
```
```python Python
workspace = client.workspaces.retrieve()
if not workspace["sandbox"]:
    ...  # Live: publish DNS for kept domains, then send to a real address.
```
## After going live

Before real users rely on it:

1. Publish the DNS records of each kept domain, including the inbound MX record where mail should arrive, and wait for it to verify.
2. Register a production [webhook endpoint](../webhooks/README.md) if you tested with `aiinbx listen`, and store its signing secret.
3. Send one email to an address you control and answer its reply, to check DNS, delivery and inbound routing for real. The [production guide](production.md#verify-the-integration-before-launch) lists the rest.

From here the workspace counts against its plan. See [Plans and billing](billing.md).
