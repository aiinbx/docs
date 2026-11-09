# Update a webhook endpoint

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/webhooks/update-webhook-endpoint)

Updates an endpoint's URL, subscriptions, routing rules, payload, or enabled state. At least one field is required. A change of `payload` applies from the next delivery attempt, retries of earlier events included; a `v1` endpoint cannot be `full`. A disabled endpoint receives nothing, and events that occur while it is off are not queued for later.

`PATCH /webhook-endpoints/{endpoint_id}`
