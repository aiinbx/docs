# domain.verified

domain.verified fires when a domain's DKIM identity is verified and it can send. Use it to offer the domain as a sender in your product.

[View on docs.aiinbx.com](https://docs.aiinbx.com/webhooks/events/domain-verified)

The domain’s DKIM identity was verified. The domain can now send.

A new domain is verified at the first check that finds its DKIM record. A domain that was [lost](domain-lost.md) is verified again only once the record has been back for an hour, every check in it finding the record.

## Payload

Every field, with an example body, is in the [`domain.verified` reference](../../api/endpoints/webhook-events/domain-verified.md), generated from the API contract.

## Handling it

Use this event to update sending readiness without continuous polling. It confirms DKIM verification, not inbound routing or every DNS setting. Check the inbound MX separately before marking receiving as ready.

Use it to flip a customer's onboarding step to done, and to start whatever you were holding back until they could send.

**[Domains](../../guides/domains.md)**

The records, and what `diagnostics` reports when one won't resolve.
