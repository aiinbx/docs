# List addresses

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/spaces/list-addresses)

Lists the addresses assigned to a space of their own, newest first. Filter by `space` for one space's addresses, or by `address` to find where one address is assigned.

A listed assignment takes effect while the domain entry it was made on is the one in service. Moving the domain to another space adds a new entry for the name; once that entry sends and receives, the address is in the domain's space again until it is assigned again, and the old assignment stays listed until the old entry is deleted, which removes it.

`GET /addresses`
