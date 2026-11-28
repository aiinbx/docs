# Create a claim link

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/sandbox/create-claim-link)

Creates a link for a person to claim a workspace nobody is a member of yet: one made with `POST /sandboxes`. Whoever opens it signs up or logs in and becomes its owner; the workspace stays a sandbox and its API keys keep working, as the owner's keys. The link works for 7 days and once, and a new one replaces the last. Nothing is mailed: pass the `url` on yourself. A workspace that has members fails with 409 `already_claimed`.

`POST /claim-links`
