# Create suppressions

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/suppressions/create-suppressions)

Adds up to 1000 addresses to a suppression list. `key` defaults to the workspace-wide `*` list and `scope` to `all`; a `scope` of `optional` blocks only sends marked as unsubscribable, so a marketing opt-out does not stop a password reset. Adding an address that is already listed updates the existing entry rather than duplicating it, and never narrows a wider scope. `space_id` puts the entries on a space's list, which stops that space's sends alone; without it they stop every space's.

`POST /suppressions`
