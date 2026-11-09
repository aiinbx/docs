# Cancel an email

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/emails/cancel-email)

Cancels a scheduled email before it is composed and sent — its status becomes `canceled` — or discards a draft, which becomes `discarded`. Either is terminal: the email cannot be put back into flight.

`POST /emails/{email_id}/cancel`
