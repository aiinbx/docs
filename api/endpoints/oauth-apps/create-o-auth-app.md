# Create an OAuth app

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/oauth-apps/create-o-auth-app)

Registers your own OAuth app so the mailbox consent screen carries your product's name rather than AI Inbx. The response returns the `redirect_uri` to register with the provider, a hosted `connect_url`, and — for Google — the `push_endpoint` your own Pub/Sub topic should publish to. The `slug` is claimed globally because it forms the connect URL. `return_urls` lists the pages of yours a connect link (`connect_url` + `/start?return_to=…`) may send a person back to; without one, only the hosted page works.

`POST /oauth-apps`
