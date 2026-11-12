# Assign an address to a space

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/spaces/assign-address)

Assigns one address of a verified domain of the workspace to a space, whatever space its domain is in: one shared domain, `anna@agents.com` in one customer's space and `bob@agents.com` in another's, with no DNS change. The domain still signs and carries the address's mail; everything else follows the address's space — its mail and threads, the `space_id` on its events and emails, its send limits, suppression lists and sender reputation. `space_id: null` assigns it to the workspace itself.

The address assigned on its own wins over its domain's space. A plus-tagged address — `anna+orders@agents.com` — goes where `anna@agents.com` goes unless it is assigned itself. An address that is not assigned is in its domain's space, as before.

Assigning an address that is already assigned moves it. Mail it already has stays in the space it was in, with that space's suppression lists; sends from it still queued or scheduled fail with `sender_unknown`, as they do when a domain moves. Mail already on its way while it moves stays in the space it was headed to. An agent login already connected to the address stays on the old space: it keeps reading the mail there and its sends are refused; connect it again for the new space. An address on no domain of the workspace fails with `sender_unknown`, one on a domain that has not verified with `409 domain_unverified`. An assigned address does not count against the plan; the emails it sends and receives do, as any email does.

`POST /addresses`
