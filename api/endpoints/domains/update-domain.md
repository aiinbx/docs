# Update domain tracking settings

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/domains/update-domain)

Updates whether opens and clicks are tracked on mail sent from the domain. Applies to every send from it that does not say otherwise in `tracking`, from the next one to leave. At least one field is required.

`PATCH /domains/{domain_id}`
