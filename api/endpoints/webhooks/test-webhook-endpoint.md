# Send a test webhook

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/webhooks/test-webhook-endpoint)

Delivers a synthetic event to the endpoint and reports what it actually did — status code, duration, error, and the first 2 KB of its response. The test event is signed like a real one, so it exercises signature verification too. `payload` is sent verbatim and does not have to match the real shape for that event type. A test event is about no message, so on a `full` endpoint the `data.email` of an `email.received` or `email.sent` test is the `email` you put in `payload`, or null.

`POST /webhook-endpoints/{endpoint_id}/test`
