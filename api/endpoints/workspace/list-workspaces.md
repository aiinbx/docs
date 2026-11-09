# List workspaces

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/workspace/list-workspaces)

Lists the workspaces the caller can act in, with its role in each. An API key reaches its own workspace alone. An OAuth token reaches every workspace it was granted, and a request with one that reaches several names the workspace it acts in with the `AIInbx-Workspace` header, set to an `id` from here; this list is the one call that needs no header. `mode` says whether the caller acts as the whole of its workspaces or as an inbox; a token connected as an inbox reaches the workspaces of its inboxes, and `inboxes` names the addresses it reads and sends as.

`GET /workspaces`
