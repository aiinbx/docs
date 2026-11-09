# Send an email

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/emails/send-email)

Sends an email — or, with `draft: true`, writes it for a person to review and send. Requires a `from` address on a verified domain or a connected mailbox, and at least one of `html` or `text`. `thread_id` continues a conversation; to answer a message AI Inbx has not seen — mail from before a migration, or from another provider — name it in `in_reply_to` and `references` instead. Recipients on a matching suppression list are dropped and reported in `suppressed`; a pacing rule that holds the message is reported in `pacing`. Pass `Idempotency-Key` so a retry after a timeout replays the original send instead of sending twice. A workspace with no plan fails with 402 `plan_required`, and one that has used its plan's emails for the month with 402 `allowance_reached`.

`POST /emails`
