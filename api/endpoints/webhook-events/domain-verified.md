# domain.verified

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/webhook-events/domain-verified)

A domain's DKIM identity was verified, and the domain can send. A new domain verifies at the first check that finds its DKIM record; one that was lost, only once the record has been back in DNS for an hour.

`POST domain.verified`

**Webhook.** The API sends this request to your endpoint.
