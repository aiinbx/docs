# Send a draft

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/emails/send-draft)

Sends a draft: at the `scheduled_at` given, else at the draft's own while it is still ahead, else now. From here it is a `scheduled` email — counted against the plan, and composed again when it goes. Anything but a draft returns 409 `not_draft`; a workspace out of emails returns 402.

`POST /emails/{email_id}/send`
