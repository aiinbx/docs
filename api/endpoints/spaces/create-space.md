# Create a space

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/spaces/create-space)

Creates a space — one per customer, typically. Domains, mailboxes, rules and suppressions are put in a space with `space_id` when they are created, and single addresses with `POST /addresses`; every email and thread inherits the space of the address it went through when that address is assigned to one, else of its domain or mailbox. Anything created without a `space_id` belongs to the workspace itself and `space_id` reads as null. API keys and webhook endpoints belong to the workspace. `external_id` is your own id for the customer: unique per workspace, so a second space for the same customer is `409 external_id_taken`, and the space can be found by it with `GET /spaces?external_id=`.

`POST /spaces`
