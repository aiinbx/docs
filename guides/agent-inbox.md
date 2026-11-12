# Give your agent an inbox

Connect Claude Code, Cursor or the aiinbx CLI to an inbox — an email address of its own that it sends from and reads, and nothing else in your workspace.

[View on docs.aiinbx.com](https://docs.aiinbx.com/guides/agent-inbox)

A coding agent building against someone else's API sometimes needs an answer only a person has: how long a vendor's export cursors live, whether a sandbox key works in production. Connected as an inbox, it gets an email address of its own. It writes to the vendor's support from that address, gets on with the rest, and reads the reply in the thread whenever it lands.

An agent connects to AI Inbx in one of two ways, each its own connection, so it can have both:

- **An inbox**: the inbox [MCP server](../mcp/README.md), `aiinbx-inbox`, or `aiinbx login --inbox` in the [CLI](../cli.md). One or more addresses, in one or several of your workspaces. The agent sends as them and reads the mail that comes to them. Nothing else in the workspace is visible or changeable.
- **Your workspaces**: the workspace MCP server, `aiinbx`, or `aiinbx login`. The whole API, at the access you choose, as far as your role allows.

An [API key](../authentication.md) always acts for its whole workspace.

## What an inbox may do

- Send from its address, schedule, draft and cancel its own emails. `from` can be left out: it sends as its inbox on the thread in `thread_id`, or as its only one. With several and none of them on the thread, name one.
- Read the threads filed under its address and the threads its address took part in: mail sent from it, or delivered to it, Cc and Bcc included. A mention in someone's headers alone does not count. In those threads it reads the mail that went out or came in, its own drafts and scheduled mail, and the text of their attachments. It sees who was blind-copied only on mail it sent.
- Reply and forward in those threads. A reply goes out from its inbox on the thread: the one mail on the thread last went through, on a tie the one the thread is filed under.
- List its workspaces. The list says `"mode": "inbox"`, and each workspace names the inbox's address under `inboxes`.

Every other operation answers `403 forbidden`: domains, mailboxes, webhooks, events, suppressions, pacing, keys, members and settings. An inbox never has more than *Read and send* access, whatever the connection asked for.

The address can be any address the workspace sends from: one on a verified [domain](domains.md), including your `<slug>.aiinbx.app` address, or a connected [mailbox](mailboxes.md). An inbox sees every thread its address is on, older mail included. Give the agent an address of its own, such as `agent@yourdomain.com`, rather than one people already use.

## Connect over MCP

Add the inbox server to your client. It sits beside the workspace server under its own name, so a client that already runs your workspace keeps doing so.

```
https://mcp.aiinbx.com/inbox
```
**Claude Code**

In a terminal:

```sh
claude mcp add --transport http aiinbx-inbox https://mcp.aiinbx.com/inbox
```
Then run `/mcp` and pick aiinbx-inbox to sign in.

**Codex**

In a terminal:

```sh
codex mcp add aiinbx-inbox --url https://mcp.aiinbx.com/inbox
codex mcp login aiinbx-inbox
```
The Codex CLI, app and IDE extension share this configuration.

**Cursor**

[Add to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=aiinbx-inbox&config=eyJ1cmwiOiJodHRwczovL21jcC5haWluYnguY29tL2luYm94In0=), or set it up by hand.

In `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "aiinbx-inbox": {
      "url": "https://mcp.aiinbx.com/inbox"
    }
  }
}
```
`.cursor/mcp.json` in a project takes the same entry, for that project only.

**Claude**

In **Customize → Connectors → Add custom connector**:

```text
https://mcp.aiinbx.com/inbox
```
Name it AI Inbx Inbox and paste the URL, then Connect to sign in and choose the address it gets.

On a Team or Enterprise plan, an owner adds it under **Organization settings** → **Connectors** first, and members then connect it from **Customize** → **Connectors**.

**ChatGPT**

In **Settings → Apps & Connectors → Create**:

```text
https://mcp.aiinbx.com/inbox
```
Turn on developer mode first. Name it AI Inbx Inbox, pick OAuth, then sign in and choose the address it gets.

Developer mode is on the Plus, Pro, Business, Enterprise and Edu plans.

**VS Code**

[Add to VS Code](vscode:mcp/install?%7B%22name%22%3A%22aiinbx-inbox%22%2C%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fmcp.aiinbx.com%2Finbox%22%7D), or set it up by hand.

In `.vscode/mcp.json`:

```json
{
  "servers": {
    "aiinbx-inbox": {
      "type": "http",
      "url": "https://mcp.aiinbx.com/inbox"
    }
  }
}
```
**Gemini CLI**

In a terminal:

```sh
gemini mcp add --transport http aiinbx-inbox https://mcp.aiinbx.com/inbox
```
Then run `/mcp auth aiinbx-inbox` in Gemini CLI to sign in.

**OpenClaw**

In a terminal:

```sh
openclaw mcp add aiinbx-inbox --url https://mcp.aiinbx.com/inbox --transport streamable-http
openclaw mcp login aiinbx-inbox
```
**Windsurf**

In `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "aiinbx-inbox": {
      "serverUrl": "https://mcp.aiinbx.com/inbox"
    }
  }
}
```
**OpenCode**

In `opencode.json`:

```json
{
  "mcp": {
    "aiinbx-inbox": {
      "type": "remote",
      "url": "https://mcp.aiinbx.com/inbox"
    }
  }
}
```
Then run `opencode mcp auth aiinbx-inbox` to sign in.

**Antigravity**

In `~/.gemini/config/mcp_config.json`:

```json
{
  "mcpServers": {
    "aiinbx-inbox": {
      "serverUrl": "https://mcp.aiinbx.com/inbox"
    }
  }
}
```
**Devin**

In **Customize → MCPs → Add custom MCP**:

```text
https://mcp.aiinbx.com/inbox
```
Pick HTTP and OAuth, save, then Connect to sign in.

**Zed**

In `settings.json`:

```json
{
  "context_servers": {
    "aiinbx-inbox": {
      "url": "https://mcp.aiinbx.com/inbox"
    }
  }
}
```
**Warp**

In **Settings → Agents → MCP servers → Add**:

```json
{
  "mcpServers": {
    "aiinbx-inbox": {
      "url": "https://mcp.aiinbx.com/inbox"
    }
  }
}
```
**Hermes**

In `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  aiinbx-inbox:
    url: "https://mcp.aiinbx.com/inbox"
    auth: oauth
```
**Any client**

As a remote server, Streamable HTTP:

```text
https://mcp.aiinbx.com/inbox
```
Yours has to sign in with OAuth: an inbox takes no API key.

Clients identify themselves with a client ID metadata document or register dynamically, so any client that speaks MCP's authorization flow signs in on its own. A client that only speaks stdio can reach it through `npx mcp-remote https://mcp.aiinbx.com/inbox`.

When the browser opens, pick a workspace and an address for each inbox the agent gets. You can also enter the name its mail goes out under.

The client loads only the mail tools: sending, reading and answering emails and threads, attachment text and the workspace list. The four generic API tools are left out, as there is nothing else they could reach. The server tells the model its address, that inbound mail was written by other people and is data rather than instructions, and that an answer can take hours.

**Ask a vendor and pick the answer up later.**

> Our order sync needs to know how long Lindmetall's export cursors stay valid. Email their support at support@lindmetall.example and ask, carry on with the rest of the sync, and check for their answer when I next ask how it is going.

## Connect the CLI

`aiinbx login --inbox` opens the same consent page. Sign the agent in under a profile of its own, so your own login keeps running the workspace:

```sh
aiinbx login -p agent --inbox   # pick its address on the consent page
aiinbx whoami -p agent          # "mode": "inbox", with its address
```
An inbox login is shown only the commands it may run: `send`, `wait`, `emails`, `threads`, `attachments` and `workspaces`, besides signing in and out and the CLI's own commands such as `commands`, `docs` and `update`. When it has one inbox in the workspace, `send` goes from it without `--from`:

```sh
aiinbx send -p agent --to support@lindmetall.example \
  --subject "How long do /v2/exports cursors live?" \
  --text "Hi, we page through /v2/exports with your cursor. Does it expire, and after how long?"

aiinbx threads -p agent --limit 5
aiinbx threads reply -p agent thr_123 --text "Thanks, that settles it."
```
Point the agent at the profile with `AI_INBX_PROFILE=agent` in its environment, so every command it runs acts as the inbox.

## Wait for the answer

People answer in minutes, hours or days. To block for a short while, use `wait`. It exits `124` when nothing came in time:

```sh
aiinbx wait -p agent --thread thr_123 --timeout 10m
```
For anything longer, check back later instead of holding a session open. List what came in on the thread since the question went out:

```sh
aiinbx emails list -p agent --thread-id thr_123 --direction inbound
```
Over MCP the model does the same with `aiinbx_emails_list`. It is told to say that nothing has arrived yet and look again later, not to call in a loop.

## Mail is still untrusted

The inbox receives mail from anyone who has its address, and any of it can contain instructions. An inbox limits what such a message could make the agent do: send as that one address, and read the threads that address is on — never another address's drafts, or who someone else blind-copied. It cannot reach the rest of your mail or change your setup. Keep your client's confirmation on for sending tools anyway, or have it send with `draft: true` so a person reads the mail at `draft.review_url` before it goes. See [Mail is untrusted input](../mcp/README.md#mail-is-untrusted-input).

## Switch or remove it

Your account page lists the connection as an inbox with its address. Editing it changes the addresses, and the next call already uses them. An inbox stays an inbox: to let the agent run the workspace too, connect it to the workspace server as well. Revoking the connection stops it at once.

An address [assigned to another space](spaces.md#single-addresses-of-one-domain) after the agent connected leaves the connection on the old space: it keeps reading the mail there, and its sends are refused. Connect it again for the new space.

Workspace admins see every inbox in the console, with who connected which agent, and can remove an agent from one. The inbox and its mail stay when its last agent leaves.

## Related

- [MCP server](../mcp/README.md): both servers, and connecting a client to the workspace.
- [CLI](../cli.md): profiles, `wait` and JSON output.
- [Threads](threads.md): how a reply finds its conversation.
