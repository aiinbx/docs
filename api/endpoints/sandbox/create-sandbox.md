# Create a sandbox

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/sandbox/create-sandbox)

Creates a sandbox workspace with no account and no members, and returns an API key for it with `full` scope; the secret is in `api_key.key` exactly once. Needs no credential. The workspace comes with its provided domain, `<slug>.aiinbx.app`, and runs every operation of the API except those that need a person or a live workspace. Give it a `name` and the slug follows it; without one it is named for you. To hand it to a person, create a claim link (`POST /claim-links`). Nobody claims it and it is deleted 14 days after its last API call. Limited to 10 an hour per IP address, an IPv6 /64 counting as one address: more fail with 429 `too_many_requests` and a `Retry-After` header. When the limit cannot be checked, the request fails with 503 `unavailable` and a `Retry-After` header.

`POST /sandboxes`
