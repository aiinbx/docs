# Delete a pacing rule

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/pacing/delete-pacing-rule)

Deletes a pacing rule. Messages it was holding become eligible to send as soon as the remaining rules allow. A `builtin` rule cannot be deleted and fails with `rule_builtin`.

`DELETE /pacing-rules/{rule_id}`
