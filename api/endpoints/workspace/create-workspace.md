# Create a workspace

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/workspace/create-workspace)

Creates a workspace for the person the request acts for, who owns it, and returns an API key for it with `full` scope; the secret is in `api_key.key` exactly once. Like every new workspace it starts as a sandbox, with its provided domain, `<slug>.aiinbx.app`, and goes live when its owner chooses a plan and presses Go live. A token keeps working in it at once: the workspace joins what its connection was granted. An API key acts for the person who created it; a key of a sandbox nobody has claimed acts for nobody and fails with 403 `forbidden` (`POST /sandboxes` creates one without an account). A person owns at most 20 sandboxes; past them the request fails with 403 `sandbox_unavailable`.

`POST /workspaces`
