# Update a space

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/spaces/update-space)

Changes the space's name or `external_id`; only the fields given change, and `external_id: null` clears it. Nothing in the space moves.

`PATCH /spaces/{space_id}`
