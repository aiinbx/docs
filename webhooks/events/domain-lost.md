# domain.lost

domain.lost fires when a verified domain's DKIM record has been missing from DNS for 72 hours: sends from it fail with domain_unverified until the record is back.

[View on docs.aiinbx.com](https://docs.aiinbx.com/webhooks/events/domain-lost)

A verified domain's DKIM record went missing from DNS and was not back for an hour within 72 hours, or the mail provider stopped signing for it. The domain no longer sends: `verified_at` is null again and sends from it fail with `domain_unverified` until the record is back.

For the 72 hours before, the domain keeps sending: [`domain.at_risk`](domain-at-risk.md) said the record was missing, and the workspace's owners and admins were mailed. A wildcard's subdomains each get their own `domain.lost`.

The event says only that the key is gone from DNS. It was removed, or replaced — a customer who moved to another platform publishes that platform's key at the same name. Which of the two is the customer's business, and the event does not tell.

## Payload

Every field, with an example body, is in the [`domain.lost` reference](../../api/endpoints/webhook-events/domain-lost.md), generated from the API contract.

## Handling it

Stop offering the domain as a sender until [`domain.verified`](domain-verified.md) comes back, and show the customer the record they are missing. Nothing else moves: the domain, its records and the mail that went through it stay exactly where they were.

**[Domains](../../guides/domains.md#staying-verified)**

Background rechecks, and what moving a domain between workspaces looks like.
