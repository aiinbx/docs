# domain.at_risk

domain.at_risk fires when a verified domain's DKIM record goes missing from DNS. The domain still sends; if the record is not back by stops_sending_at, 72 hours later, the domain is lost.

[View on docs.aiinbx.com](https://docs.aiinbx.com/webhooks/events/domain-at-risk)

A verified domain's DKIM record is missing from DNS. The domain still sends, but receivers can't check its signature, so mail from it may be rejected or land in spam. `data.dkim_missing_since` says when the record was first found missing and `data.stops_sending_at` when the domain is lost if it is not back: 72 hours later.

One check that misses the record is not enough: the event fires when two of the last three checks miss it, even with one between that found it — as when one of the zone's nameservers lacks the record. `dkim_missing_since` is the first of the two.

The event is sent for the domain that holds the records. A wildcard's subdomains get no `domain.at_risk` of their own; each gets [`domain.lost`](domain-lost.md) if the wildcard is lost.

## Payload

Every field, with an example body, is in the [`domain.at_risk` reference](../../api/endpoints/webhook-events/domain-at-risk.md), generated from the API contract.

## Handling it

Show the customer the record they are missing and the time in `stops_sending_at`. Once the record has been back for an hour, [`domain.restored`](domain-restored.md) follows and nothing else changes. If it is not back in time, [`domain.lost`](domain-lost.md) follows and sends from the domain fail.

**[Domains](../../guides/domains.md#staying-verified)**

Background rechecks, and what happens during the 72 hours.
