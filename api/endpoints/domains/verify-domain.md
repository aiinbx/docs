# Verify domain DNS records now

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/domains/verify-domain)

Checks the domain's DNS records against live DNS now. `verified_at` stays null until the sending identity’s DKIM verification succeeds; inbound MX, SPF, and DMARC readiness are separate. DNS propagation usually takes minutes and occasionally hours, so poll at a sane interval or subscribe to the `domain.verified` webhook instead. Domains are also rechecked in the background: a verified domain whose DKIM record stops resolving gets `dkim_missing_since`, keeps sending, and `domain.at_risk` is sent; `domain.restored` follows if the record is back, and has held for an hour, by `stops_sending_at`, otherwise it loses `verified_at` and `domain.lost` is sent.

`POST /domains/{domain_id}/verify`
