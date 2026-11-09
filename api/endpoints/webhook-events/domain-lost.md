# domain.lost

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/webhook-events/domain-lost)

A verified domain stopped authenticating: SES stopped signing with its key, or its DKIM record went missing from DNS and was not back for an hour within 72 hours. It can no longer send. A wildcard's subdomains each get their own.

`POST domain.lost`

**Webhook.** The API sends this request to your endpoint.
