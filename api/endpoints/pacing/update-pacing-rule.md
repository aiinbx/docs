# Update a pacing rule

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/pacing/update-pacing-rule)

Updates a pacing rule. At least one field is required. Setting `enabled` to false parks a rule without losing its configuration, which is the quickest way to confirm which rule is holding mail. A `builtin` rule cannot be changed and fails with `rule_builtin`. Setting `enabled` to true on a parked rule takes one of the plan's active custom pacing rules again, and fails with 402 `allowance_reached` when none is left.

`PATCH /pacing-rules/{rule_id}`
