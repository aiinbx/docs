# Retry webhook deliveries

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/webhooks/retry-webhook-deliveries)

Replays up to 100 deliveries. A replay carries the same event `id` as the original, so a handler that deduplicates on it stays correct.

`POST /webhook-endpoints/{endpoint_id}/deliveries/retry`
