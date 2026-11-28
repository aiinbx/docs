# Go live

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/workspace/go-live-workspace)

Makes a sandbox a live workspace: from then on its mail is delivered and received for real and billed to its plan. Everything the sandbox produced is removed first: emails, threads, attachments, events and webhook deliveries, suppressions, pacing counters, and every space with everything in it. With `keep`, the default, API keys, webhook endpoints, domains, pacing rules and settings stay; kept domains go back to pending and verify through their DNS records like any domain added to a live workspace. The workspace needs a plan, a free one included; without one the request fails with 402 `plan_required`. A sandbox nobody has claimed fails with 403 `forbidden`: a person claims it first, through a claim link (`POST /claim-links`). It returns at once with `going_live` true, and the workspace stays a sandbox until the removal is done, usually within minutes. Meanwhile every API change fails with 409 `going_live`, and reads answer what the workspace keeps outside spaces, such as domains, API keys and webhook endpoints, while its mail and everything in its spaces are being removed. A workspace that is already live fails with 409 `already_live`. Admin only.

`POST /workspace/go-live`
