# Diagnose domain DNS configuration

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/domains/retrieve-domain-diagnostics)

Reads the domain's live DNS zone and reports what is wrong with it — a duplicate SPF record, a permissive policy, a missing return path. Each finding carries a severity, an explanation, the fix, and the records it was drawn from. Nothing is stored; the result always reflects DNS as it is right now.

`GET /domains/{domain_id}/diagnostics`
