# List pacing rules

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/pacing/list-pacing-rules)

Lists the workspace's pacing rules, both sending-hours and rate-limit kinds. Rules with `builtin: true` came with the workspace: they cap mail sent from the [provided domain](../../../guides/domains.md#your-provided-domain) and cannot be changed or removed.

`GET /pacing-rules`
