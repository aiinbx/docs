# List webhook deliveries

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/webhooks/list-webhook-deliveries)

Lists deliveries to an endpoint, filterable by state. Each carries the event envelope it sends and every attempt made: timestamp, status code, duration, error, and the beginning of your response, which is usually what explains a rejection. `body` is what is stored. For a `full` endpoint that is the envelope without `data.email`: the email is read from the message each time a delivery is sent and is not kept with it. It is null for a `v1` endpoint — its legacy body is built from the message at send time, not stored — and for a delivery nothing was sent for.

`GET /webhook-endpoints/{endpoint_id}/deliveries`
