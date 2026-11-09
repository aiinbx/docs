# Create a pacing rule

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/pacing/create-pacing-rule)

Creates a pacing rule. An `hours` rule holds mail outside the weekly windows you declare, evaluated in the rule's own timezone. A `limit` rule caps sends per rolling window against a scope — the rule as a whole, or per sender address, sending domain, recipient, or recipient domain. An empty `match` array applies the rule to every send. `space_id` makes the rule read one space's mail alone; workspace rules apply to every space and stack on top. An enabled rule counts against the plan's active custom pacing rules: with none left the request fails with 402 `allowance_reached`, and a workspace with no plan with 402 `plan_required`. A rule created with `enabled` false takes nothing until it is switched on.

`POST /pacing-rules`
