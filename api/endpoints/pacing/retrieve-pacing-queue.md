# Retrieve the pacing queue

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/pacing/retrieve-pacing-queue)

Returns a snapshot of everything pacing is currently holding: a sample of queued messages, the rules in effect, and per-rule counts of how many messages each matched and how many it is holding. The per-rule counts are the fastest way to identify which rule is responsible.

`GET /pacing-queue`
