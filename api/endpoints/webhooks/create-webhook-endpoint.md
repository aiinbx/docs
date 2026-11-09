# Create a webhook endpoint

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/webhooks/create-webhook-endpoint)

Registers an organization-wide endpoint and returns its signing secret exactly once, in the `secret` field: no later read shows it again. A secret that was lost, or has leaked, is replaced with `POST /webhook-endpoints/{endpoint_id}/rotate-secret`, which keeps the old one verifying for a grace period while you deploy the new one. `subscriptions` must name at least one event type. Setting `payload` to `full` puts the whole email on `email.received` and `email.sent`, as `data.email`, so a handler does not have to fetch it. Optional routing rules narrow delivery by sender or recipient: a matching `block` rule always wins, and an empty allow list means no opinion. The endpoint receives every space's events, each stamped with its `space_id`.

`POST /webhook-endpoints`
