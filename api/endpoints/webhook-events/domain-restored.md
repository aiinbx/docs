# domain.restored

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/webhook-events/domain-restored)

A domain's missing DKIM record has been back in DNS for an hour, with every check finding it, before the domain was lost; it signs as before. Sent only for the domain that holds the records: a wildcard's subdomains get none of their own.

`POST domain.restored`

**Webhook.** The API sends this request to your endpoint.
