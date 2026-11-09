# List events

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/events/list-events)

Lists the workspace's events, newest first — every event, recorded when it happens, whether or not a webhook endpoint subscribes to its type. Each is the envelope a webhook delivers, with the same `id`, so a handler that deduplicates on it can read from here and from its endpoint alike. Filter by `type`, repeated for any of several, or by `email_id` for what happened to one email. To catch up after missed webhooks, walk forward: `order=asc` with `after` set to the `created_at` of the last event you processed. `data` is the summary every endpoint receives; the `data.email` a `full` endpoint is sent is read when a delivery goes out and not kept, so read the email by its `email_id`. Test events sent to an endpoint are not listed.

`GET /events`
