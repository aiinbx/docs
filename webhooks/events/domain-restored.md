# domain.restored

domain.restored fires when a domain's missing DKIM record has been back in DNS for an hour before the domain was lost. The domain signs as before.

[View on docs.aiinbx.com](https://docs.aiinbx.com/webhooks/events/domain-restored)

A domain's missing DKIM record has been back in DNS for an hour before the domain was lost. It signs as before, and `dkim_missing_since` and `stops_sending_at` on the domain are null again.

One check that finds the record is not enough: it is rechecked every 15 minutes once it is back, and the event fires when every check for an hour has found it. A check that misses it in that hour starts the hour again, and `stops_sending_at` stays where it was.

The event is sent for the domain that holds the records. A wildcard's subdomains get no `domain.restored` of their own.

## Payload

Every field, with an example body, is in the [`domain.restored` reference](../../api/endpoints/webhook-events/domain-restored.md), generated from the API contract.

## Handling it

Clear whatever warning [`domain.at_risk`](domain-at-risk.md) put up. The domain kept sending the whole time, so there is nothing to resume.

**[Domains](../../guides/domains.md#staying-verified)**

Background rechecks, and what happens during the 72 hours.
