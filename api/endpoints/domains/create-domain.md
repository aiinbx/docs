# Create a domain

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/domains/create-domain)

Registers a sending domain and returns the DNS records to publish: SPF, DKIM, DMARC, a return path, and an MX record for receiving. `region` selects the regional mail infrastructure for this domain; it can be changed with `PATCH /domains/{domain_id}` until the domain is verified.

A wildcard — `*.acme.dev` — is verified once and covers every subdomain: the MX record goes at `*.acme.dev`, the rest at `acme.dev`, and the apex keeps whatever mail setup it has. A name one label under a wildcard of yours — `hi.acme.dev` under `*.acme.dev` — is created as a subdomain of it: no records, verified as soon as the wildcard is, in whatever `space_id` you give it. That is how a platform hands each customer an address of their own without touching DNS again. To give customers single addresses of one shared domain instead, assign each address to its customer's space with `POST /addresses`.

A domain name can be connected in several spaces and workspaces, but sending and receiving are enabled for the one whose DKIM key is published. To move them, add the domain where it should go and publish its DNS records. The next verification check moves them; existing mail stays where it was. Duplicate apex or wildcard identities within one space fail with `domain_taken`.

Every workspace already has one domain it did not add: `<slug>.aiinbx.app`, listed with `provided: true`. It is verified through our zone, has no records and cannot be deleted; a name under `aiinbx.app` cannot be added by hand and fails with `reserved_domain`.

A domain of its own counts against the plan's custom domains — a subdomain under a wildcard and the provided domain do not. With none left the request fails with 402 `allowance_reached`, and a workspace with no plan with 402 `plan_required`.

`track_opens` and `track_clicks` set the domain's `tracking`; what they leave out starts as the workspace's tracking defaults in the console say — off unless they are set there — or, on a subdomain, as its wildcard's. Change either afterwards with `PATCH /domains/{domain_id}`.

`POST /domains`
