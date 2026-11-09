# domain.at_risk

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/webhook-events/domain-at-risk)

A verified domain's DKIM record is missing from DNS: two of its last three checks did not find it, even with one between that did. It still sends; if the record is not back, and held for an hour, by `stops_sending_at` (72 hours after the first miss) the domain is lost. Sent only for the domain that holds the records: a wildcard's subdomains get none of their own.

`POST domain.at_risk`

**Webhook.** The API sends this request to your endpoint.
