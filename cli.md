# CLI

The aiinbx command line: send and read mail, call every API resource, forward webhooks to localhost without a tunnel, and check a domain's DNS.

[View on docs.aiinbx.com](https://docs.aiinbx.com/cli)

`aiinbx` runs your AI Inbx workspace from a terminal, a script or an agent. Every API operation is a command. A few jobs only a local tool can do have commands of their own: sending files and React Email templates, forwarding webhook events to `localhost`, blocking until an email arrives, and checking a domain's DNS.

```sh
aiinbx login
aiinbx send --from you@yourdomain.com --to someone@example.com \
  --subject "Hello" --text "Sent from the terminal"
```
## Install

The CLI is a single executable. Nothing else, not Node and not Bun, has to be installed to run it.

```sh macOS & Linux
curl -fsSL https://aiinbx.com/install.sh | sh
```
```sh Homebrew
brew install aiinbx/tap/aiinbx
```
```sh npm
npm install -g @aiinbx/cli
```
```powershell Windows
powershell -c "irm https://aiinbx.com/install.ps1 | iex"
```
To run it once without installing: `npx @aiinbx/cli --help`.

The CLI looks for a newer release once a day and, while one is out, says so after each command at a terminal. `aiinbx update` installs it the way the CLI was installed: through npm, pnpm, Bun, Yarn or Homebrew, or, for the install script's copy, by downloading the release, checking it against its checksums and replacing itself.

`aiinbx uninstall` removes the CLI the same way, along with the PATH entry the install script added. It signs out of every profile and deletes the config directory; `--keep-config` keeps them.

## Sign in

```sh
aiinbx login                  # opens the browser
aiinbx login --device         # a code to approve on another device (SSH, containers)
echo "$AI_INBX_API_KEY" | aiinbx login --with-api-key
aiinbx whoami
```
A login acts for your account in the workspaces you grant it. When it reaches more than one, pick the one commands act in with `aiinbx workspace use <slug>`, or pass `--workspace` for a single command. An [API key](authentication.md) belongs to one workspace and needs neither.

`aiinbx login --inbox` signs in as an inbox instead: one or more addresses it sends from and reads, for an agent. It is a connection of its own, so keep it in its own profile (`aiinbx login --inbox -p agent`) beside your workspace login. `aiinbx whoami` then says `"mode": "inbox"` with its addresses, help lists only the commands an inbox may run, and `send` goes from the inbox without `--from`. See [Give your agent an inbox](guides/agent-inbox.md).

Credentials are taken from `--api-key` first, then `AI_INBX_API_KEY`, then the signed-in profile. A profile's secret is kept in the macOS Keychain, the Secret Service on Linux, or DPAPI on Windows. `-p <name>` or `AI_INBX_PROFILE` picks a profile, and `aiinbx auth switch <name>` makes one the default.

## Commands

Every API operation is a command, `aiinbx <resource> <operation>`. Path parameters are arguments and fields are flags. A resource on its own lists it:

```sh
aiinbx threads --limit 5
aiinbx threads retrieve thr_123
aiinbx threads reply thr_123 --text "Thanks, on it"
aiinbx domains create --name yourdomain.com --region eu-central-1
aiinbx events list --type email.bounced --type email.complained --limit 50
aiinbx webhook-endpoints create --url https://example.com/hooks --subscriptions email.received
aiinbx attachments download att_123 --output invoice.pdf
```
- Repeatable flags take one value each: `--to a@x.com --to b@x.com`.
- Booleans are switches: `--track-opens`, `--no-track-opens`.
- Nested fields take JSON: `--tracking '{"opens":false}'`.
- `--data` takes the whole body as JSON, `@file.json`, or `-` for stdin. Flags are laid over it.
- `'[]'` sends an empty list: `aiinbx emails update eml_123 --cc '[]'` clears it.
- Lists take `--limit` and `--cursor`. `--all` fetches every page.
- Commands that cannot be undone ask first, or need `--yes` without a terminal.

The commands are generated from the same contract as the [API reference](api/README.md) and the SDKs, so they cannot drift from the API. `aiinbx <command> --help` shows every flag with examples.

## Send

```sh
aiinbx send --from you@yourdomain.com --to a@example.com --subject Report \
  --html-file report.html --text-file report.txt --attach report.pdf

cat notes.md | aiinbx send --from you@yourdomain.com --to a@example.com \
  --subject Notes --text-file -

aiinbx send --from you@yourdomain.com --to a@example.com --subject Welcome \
  --react emails/welcome.tsx --props '{"name":"Ada"}' \
  --scheduled-at 2026-10-01T09:00:00Z
```
```text
Idempotency key 4e306697-7390-43c4-8e61-cb5a455ff376
✓ Sent eml_5c2e… to a@example.com (queued)
```
`--react` renders a [React Email](integrations/react-email.md) component with the `react` and `@react-email/render` installed in the template's own project, or the CLI's own copy where the project has none. Every send carries an [idempotency key](reference/conventions.md), `--idempotency-key` or a generated one, so the CLI's own retries never send twice. Running the command again is a new email unless you pass the same `--idempotency-key`.

### Drafts and scheduled sends

`--draft` stores the email instead of sending it, and `--scheduled-at` holds it until then. Both work on `send`, `threads reply` and `threads forward`:

```sh
aiinbx threads reply thr_123 --text "Draft for Ada to check" --draft
# ✓ Drafted eml_9d4a… to ada@example.com — review it at https://aiinbx.com/…

aiinbx emails update eml_9d4a… --text "Checked and fixed"
aiinbx emails send-draft eml_9d4a…                # or --scheduled-at to send it later
aiinbx emails cancel eml_7b1c…                    # a scheduled email that has not gone out
```
See [Drafts](guides/drafts.md) and [Scheduling](guides/scheduling.md).

## Listen: webhooks on localhost

```sh
aiinbx listen --forward-to localhost:3000/api/webhooks
```
```text
Ready. Forwarding every event to http://localhost:3000/api/webhooks
Signing secret whsec_jeMeEt8G… (aiinbx listen --print-secret)
Ctrl-C to stop.
18:54:11  email.received        evt_a1b2c3…  → 200  7 ms
18:54:12  email.delivered       evt_b2c3d4…  → 200  1 ms
18:54:40  email.bounced         evt_c3d4e5…  → 500  3 ms
```
`listen` forwards your workspace's events to a URL on your machine. Each one arrives as a real [webhook delivery](webhooks/README.md) would: the same JSON body, the same `AIInbx-Event-Id` and `AIInbx-Signature` headers, the same [signature scheme](webhooks/verifying.md). There is no tunnel to run and no endpoint to register. The CLI follows the [events API](api/README.md) live and posts each event the moment it is recorded.

The deliveries are signed with a secret of your profile's own. It stays the same from run to run, so put it in your app's environment once:

```sh
aiinbx listen --print-secret
# whsec_…
AI_INBX_WEBHOOK_SECRET=$(aiinbx listen --print-secret) npm run dev
```
Your handler's `verifyWebhookRequest(request, process.env.AI_INBX_WEBHOOK_SECRET)` then accepts local deliveries unchanged. In production the same code gets the endpoint's secret.

```sh
aiinbx listen --forward-to localhost:3000/api/webhooks \
  --events email.received,email.bounced \
  --since 1h
```
- `--events` forwards only these types.
- `--since` replays from an ISO 8601 time or a duration ago (`15m`, `2h`) before it goes on listening. Without it, `listen` starts with what happens next.
- At a terminal it prints a line per event: its time, type and id, what your server answered, and how long it took. Anywhere else it prints one JSON object per line: `{"event_id","type","created_at","status","latency_ms","error"}`.
- A server that is down or answers an error is logged, and the next event still goes out. `listen` is not retried like a real endpoint.

> [!NOTE]
> **The summary body**
>
> Deliveries carry the body every endpoint receives. The `data.email` that an endpoint with `payload: "full"` is also sent is not included. Read the email by `data.email_id`.

## Wait: block until an email arrives

```sh
aiinbx wait --to qa@yourdomain.com --subject "verification code" --timeout 1m
```
```text
id            eml_8c1e4f…
thread_id     thr_3f9a2c…
from          Acme <no-reply@acme.com>
to            qa@yourdomain.com
subject       Your verification code
created_at    2026-09-26T16:54:11.706Z
verification
  code  482913
  link  —
text          Your code is 482913.
```
`wait` blocks until a matching email arrives, prints it and exits `0`. When `--timeout` passes first (two minutes by default), it exits `124`, as `timeout` does. If the API never answered at all, it exits `1` with the API's error: an outage, not a missing email. It is made for agents and end-to-end tests: sign up, wait for the verification email, read the code. A verification email carries it as [`verification`](guides/receiving.md#what-automated-mail-asks-for), the code and the link picked out of the mail, and `wait` shows that first.

| Flag                      | Matches                                                   |
| ------------------------- | --------------------------------------------------------- |
| `--from <text>`           | Part of the sender: an address, a domain or a name        |
| `--to <text>`             | Part of a recipient in `to`                               |
| `--subject <text>`        | Part of the subject                                       |
| `--thread <id>`           | Only this thread                                          |
| `--direction <direction>` | `inbound` (the default) or `outbound`                     |
| `--since <time>`          | Count mail from this ISO 8601 time or duration ago (`5m`) |

Matching ignores case and extra spaces. Piped or with `--json`, the output is the whole email as `GET /emails/{id}` returns it, `verification` included:

```sh
started=$(date -u +%FT%TZ)
curl -s -X POST https://app.example.com/signup -d email=qa+$RUN@yourdomain.com
code=$(aiinbx wait --to "qa+$RUN@yourdomain.com" --since "$started" --json \
  | jq -er .verification.code)
```
> [!TIP]
> **Start the clock before the trigger**
>
> Without `--since`, `wait` counts only mail that arrives after it starts. If the email can be sent before `wait` runs, take the time first and pass it as `--since`, as above. A unique [plus address](guides/receiving.md) per run keeps parallel tests from reading each other's mail.

## Doctor

```sh
aiinbx doctor
```
```text
✓ CLI               0.6.0, the latest
✓ Credentials       profile default, a login
✓ API               https://api.aiinbx.com/api/v2 answered in 84 ms
✓ Workspace         Acme (acme)
✓ yourdomain.com    verified, eu-central-1
! mail.example.com  not verified
  → aiinbx doctor mail.example.com
```
`doctor` checks, in order: the CLI's version and whether a newer one is out, where the credentials come from, whether the API answers and how fast, the workspace, and each domain's verification state.

```sh
aiinbx doctor mail.example.com
```
```text
✗ mail.example.com  not verified
  → Publish the records below, then: aiinbx domains verify dom_2b7e…
✓ DKIM CNAME        k1._domainkey.mail.example.com
✗ DKIM CNAME        k2._domainkey.mail.example.com missing
  → Publish CNAME k2._domainkey.mail.example.com with the value k2.dkim.amazonses.com
✓ INBOUND MX        mail.example.com
✗ Two SPF records
  mail.example.com publishes two SPF records, so receivers treat SPF as failed.
  → Merge them into one TXT record: v=spf1 include:_spf.google.com include:amazonses.com ~all
```
Given a domain's name or id, it lists each DNS record the domain needs and whether DNS has it. It also runs the [live zone check](guides/domains.md) for duplicate SPF records, permissive policies, a missing return path and more. Every failure comes with its exact fix, such as the record to publish or the command to run.

`doctor` exits `1` when any check fails. With `--json` it prints `{ ok, checks }`. Each check has an `id`, a `status` (`ok`, `warn`, `fail` or `info`), a `title`, a `detail`, and a `fix` and `evidence` where there are any.

## JSON output and exit codes

At a terminal the CLI prints tables, key/value views and spinners. Anywhere else, whether a pipe, CI or an agent, or with `--json`, it prints the API's JSON on stdout and never prompts. So does any `CI` variable other than `false` or `0`. There is no `--quiet`: in JSON mode stdout carries only the answer, so `jq -r .id` picks out an id.

Errors go to stderr. In JSON mode an error is one object:

```json
{
  "error": {
    "code": "domain_unverified",
    "message": "...",
    "request_id": "req_..."
  }
}
```
| Exit code | Meaning                                                                                    |
| --------- | ------------------------------------------------------------------------------------------ |
| `0`       | It worked.                                                                                 |
| `1`       | The API refused or never answered, the command line was wrong, or a `doctor` check failed. |
| `2`       | No credentials, or the API does not accept them: sign in again.                            |
| `124`     | `wait` timed out.                                                                          |
| `130`     | A prompt or a `wait` was cancelled.                                                        |

## For agents

Without a terminal, `login` never waits on a browser. It prints a device code as JSON and exits, and `--complete` waits for the approval:

```sh
$ aiinbx login --device --non-interactive
{
  "verification_uri_complete": "https://aiinbx.com/device?user_code=WDJBMJHT",
  "user_code": "WDJBMJHT",
  "expires_in": 1800,
  "next": "aiinbx login --complete"
}
$ aiinbx login --complete
```
Or skip the login and pass a key: `AI_INBX_API_KEY=... aiinbx emails list`.

`aiinbx commands` prints the whole command tree as JSON, with every argument and flag and the API operation behind each command. An agent can read it once instead of parsing `--help`.

The `aiinbx-cli` [agent skill](skills.md) covers the non-interactive rules, auth, and the `listen`, `wait` and `doctor` workflows:

```sh
npx skills add aiinbx/skills --skill aiinbx-cli
```
## In CI

Give CI an [API key](authentication.md) in `AI_INBX_API_KEY`, not in `--api-key`: a flag shows up in process lists and logs. An end-to-end test that waits for a verification email, in GitHub Actions:

```yaml
- run: npm install -g @aiinbx/cli
- name: Sign up and read the code
  shell: bash
  env:
    AI_INBX_API_KEY: ${{ secrets.AI_INBX_API_KEY }}
    INBOX: qa+${{ github.run_id }}@yourdomain.com
  run: |
    started=$(date -u +%FT%TZ)
    npm run e2e:signup -- "$INBOX"
    aiinbx wait --to "$INBOX" --since "$started" --timeout 2m --json \
      | jq -er .verification.code > code.txt
```
When no email comes, `wait` exits `124` and fails the step. `shell: bash` runs with `pipefail`, so the pipe after it does not hide that, and `jq -e` fails it when the email holds no code.

## Environment

| Variable                     | What it does                                                |
| ---------------------------- | ----------------------------------------------------------- |
| `AI_INBX_API_KEY`            | The API key to use, over any login.                         |
| `AI_INBX_PROFILE`            | The profile to act as.                                      |
| `AI_INBX_WORKSPACE`          | The workspace a login acts in.                              |
| `AI_INBX_API_URL`            | The API base URL (`https://api.aiinbx.com/api/v2`).         |
| `AI_INBX_AUTH_URL`           | Where logins go (`https://aiinbx.com`).                     |
| `AI_INBX_CREDENTIALS_STORE`  | `file` keeps secrets in `credentials.json`, not a keychain. |
| `AI_INBX_NO_UPDATE_NOTIFIER` | Never look for a newer release.                             |
| `NO_COLOR`                   | Print without colour.                                       |

## Shell completion

```sh
eval "$(aiinbx completion bash)"      # ~/.bashrc
eval "$(aiinbx completion zsh)"       # ~/.zshrc, after compinit
aiinbx completion fish > ~/.config/fish/completions/aiinbx.fish
aiinbx completion powershell | Out-String | Invoke-Expression   # $PROFILE
```
