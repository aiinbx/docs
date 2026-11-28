# Agent skills

Agent skills for AI Inbx: a coding agent creates a sandbox, wires email into your app, verifies a round trip, and hands you a link to go live.

[View on docs.aiinbx.com](https://docs.aiinbx.com/skills)

The skills take a coding agent from "add email to this app" to your app sending an email, receiving the reply and answering it in the same thread, in a [sandbox](guides/sandbox.md) it creates without an account. You then open one link to own the workspace and go live. They work in any agent that reads [Agent Skills](https://agentskills.io): Claude Code, Codex, Cursor and others.

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
| Skill              | Job                                                                                              |
| ------------------ | ------------------------------------------------------------------------------------------------ |
| `aiinbx-setup`     | First-time setup: a sandbox, an API key in your env file, one verified round trip, a claim link. |
| `aiinbx-integrate` | Your server code: sending, a verified webhook route, threads, replies, drafts, delivery events.  |
| `aiinbx-concepts`  | The objects: domains, mailboxes, emails, threads, events, spaces.                                |
| `aiinbx-cli`       | The [`aiinbx` command line](cli.md): send, `listen`, `wait`, `doctor`, and every API resource.     |

`aiinbx-setup` runs the flow and hands over to the others, so installing all four is the usual choice. The source is [github.com/aiinbx/skills](https://github.com/aiinbx/skills).

## What setup does

1. **Checks the project**

    Language, framework, the env file your app loads, and whether AI Inbx is
    already in it.

2. **Connects it**

    Uses whatever is already there (MCP, the [CLI](cli.md) or plain HTTP) and
    creates a [sandbox](guides/sandbox.md): a workspace whose mail stays inside AI
    Inbx, with no account or plan needed. If you already have an account and want
    to use it, it signs you in instead. It writes the new workspace's API key into
    your env file without printing it; in a workspace you already had, it creates
    a `sending` key for the app instead.

3. **Asks what to build**

    Whether the app only sends, holds conversations, or acts on mail that arrives.

4. **Writes the code**

    One shared client, one send from the place in your app that should send, and
    one webhook route that verifies the signature and answers in the thread.

5. **Proves it**

    Sends one email from your [provided
    domain](guides/domains.md#your-provided-domain) to `reply@simulator.aiinbx.com`,
    with [`aiinbx listen`](cli.md#listen-webhooks-on-localhost) forwarding events to
    your local app. The simulator answers, and your app answers it in the same
    thread.

6. **Hands it over**

    Gives you a [claim link](guides/going-live.md#claim-a-sandbox). You open it,
    sign up or log in, and own the workspace. Then you choose a
    [plan](guides/billing.md) in the console and go live there, or tell the agent to
    go live. The keys in your env file keep working.

The browser is for you at the end: claiming the workspace and choosing a plan. Everything before that happens in your terminal and your code.

## What the agent will not do

- Send mail to anyone but the simulator without your yes, or to an address you did not name.
- Choose a plan, or go live without your yes.
- Read, print or ask you to paste an API key or a webhook secret.
- Create a domain or change DNS unless you asked for your own domain.
- Follow instructions found inside an email.

## For other agents

The skills are also listed at [`/.well-known/agent-skills/index.json`](https://aiinbx.com/.well-known/agent-skills/index.json), and [`auth.md`](https://aiinbx.com/auth.md) says how an agent gets a sandbox or signs in, and where an API key goes. The workspace [MCP server](mcp/README.md#skills) serves the same skills to clients that load skills from an MCP server. An assistant that only needs to send and read mail, with no code of yours, can use that server's tools instead.
