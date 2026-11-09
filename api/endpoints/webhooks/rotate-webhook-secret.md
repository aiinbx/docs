# Rotate a webhook signing secret

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/webhooks/rotate-webhook-secret)

Issues a new signing secret and returns it, once — the only way to get a secret after the endpoint was created. The previous one keeps verifying for `grace_seconds` (a day by default, a week at most): every delivery in that window is signed with both, so a deployment can roll over without dropping events, and `previous_secret_expires_at` says when the overlap ends. Set the grace period to 0 only when a secret has leaked and the exposure matters more than the gap.

`POST /webhook-endpoints/{endpoint_id}/rotate-secret`
