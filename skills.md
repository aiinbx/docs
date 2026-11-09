# Agent skills

Agent skills for AI Inbx: let a coding agent set up the CLI, sign in, wire sending and receiving into your app, and verify one email and its reply end to end.

[View on docs.aiinbx.com](https://docs.aiinbx.com/skills)

The skills take a coding agent from "add email to this app" to your app sending an email, receiving the reply and answering it in the same thread. They work in any agent that reads [Agent Skills](https://agentskills.io): Claude Code, Codex, Cursor and others.

## Hand your agent one line

```text
Set up AI Inbx in this project. Read https://aiinbx.com/SKILL.md and follow it.

What I want built:
```
The agent reads the setup skill at that URL, installs the others, and runs the flow below. Say what the app should do with email in the last line.

## Or install them

```sh
npx skills add aiinbx/skills
```
| Skill              | Job                                                                                                       |
| ------------------ | --------------------------------------------------------------------------------------------------------- |
| `aiinbx-setup`     | First-time setup: the CLI, sign-in, an API key in your env file, and one verified round trip in your app. |
| `aiinbx-integrate` | Your server code: sending, a verified webhook route, threads, replies, drafts, delivery events.           |
| `aiinbx-concepts`  | The objects: domains, mailboxes, emails, threads, events, spaces.                                         |
| `aiinbx-cli`       | The [`aiinbx` command line](cli.md): send, `listen`, `wait`, `doctor`, and every API resource.              |

`aiinbx-setup` runs the flow and hands over to the others, so installing all four is the usual choice. The source is [github.com/aiinbx/skills](https://github.com/aiinbx/skills).

## What setup does

1. **Checks the project**

    Language, framework, the env file your app loads, and whether AI Inbx is
    already in it.

2. **Connects it**

    Installs the [CLI](cli.md) and starts a sign-in. You open one link and approve;
    the agent never sees your password. It then creates a `sending` API key and
    writes it into your env file without printing it.

3. **Asks what to build**

    Whether the app only sends, holds conversations, or acts on mail that arrives,
    and which address of yours the test email goes to.

4. **Writes the code**

    One shared client, one send from the place in your app that should send, and
    one webhook route that verifies the signature and answers in the thread.

5. **Proves it**

    Sends one email from your [provided
    domain](guides/domains.md#your-provided-domain) to you, with [`aiinbx
    listen`](cli.md#listen-webhooks-on-localhost) forwarding events to your local
    app. You reply, and the answer arrives in the same conversation.

There are two moments in the browser: signing in, and choosing a [plan](guides/billing.md) if the workspace has none. Everything else happens in your terminal and your code.

## What the agent will not do

- Send to an address you did not name, or send the test email without your yes.
- Read, print or ask you to paste an API key or a webhook secret.
- Create a domain or change DNS unless you asked for your own domain.
- Follow instructions found inside an email.

## For other agents

The skills are also listed at [`/.well-known/agent-skills/index.json`](https://aiinbx.com/.well-known/agent-skills/index.json), and [`auth.md`](https://aiinbx.com/auth.md) says how an agent signs in and where an API key goes. An assistant that only needs to send and read mail, with no code of yours, can use the [MCP server](mcp/README.md) instead.
