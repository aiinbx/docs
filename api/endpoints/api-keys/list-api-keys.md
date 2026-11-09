# List API keys

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/api-keys/list-api-keys)

Lists the workspace's API keys, newest first. Keys are returned as metadata only — the prefix, scope, creation time, and `last_used_at`; the secret itself is never readable after creation. Admin only.

`GET /api-keys`
