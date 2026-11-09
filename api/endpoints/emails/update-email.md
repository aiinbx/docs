# Update an email

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/emails/update-email)

Changes an email that has not gone out — a `draft` or a `scheduled` one: its recipients, subject, bodies, attachments or send time. Fields left out stay as they are. The email is composed again when it goes, so what is sent is what it says by then. One that has begun sending, or was canceled or discarded, returns 409 `not_scheduled`.

`PATCH /emails/{email_id}`
