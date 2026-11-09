# Delete a domain

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/domains/delete-domain)

Deletes a domain and stops future sends from it. Remove the published DNS records afterwards. A wildcard's subdomains are deleted with it, in whatever space they sit; a subdomain is removed on its own, with nothing to unpublish. The provided domain cannot be deleted and fails with `domain_provided`.

`DELETE /domains/{domain_id}`
