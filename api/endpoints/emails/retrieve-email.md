# Retrieve an email

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/emails/retrieve-email)

Returns one email in full: both bodies, every header, attachments, the delivery event history, and tracked engagements. Pass `include=attachment_content` to inline each attachment's prepared text, which saves a request per attachment before handing a message to a model.

`GET /emails/{email_id}`
