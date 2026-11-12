# Delete a space

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/spaces/delete-space)

Marks the space for deletion and starts its teardown in the background. The space immediately stops accepting work and no longer appears in the API; its domains then come off DNS verification and sending, its mailboxes are disconnected, and its emails, threads, rules and suppressions are removed. Addresses assigned to the space are unassigned; one whose domain lives in another space is in that domain's space again from its next email on. A wildcard domain in the space is deleted with its subdomains, including any filed in other spaces, and a domain deleted with the space takes the assignments of its addresses with it, whatever space they are in. Admin only.

`DELETE /spaces/{space_id}`
