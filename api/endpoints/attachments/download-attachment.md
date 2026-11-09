# Download an attachment

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/attachments/download-attachment)

Redirects to a short-lived signed URL for the attachment's original bytes. Follow the redirect; the signed URL expires, so resolve it on demand rather than storing it.

`GET /attachments/{attachment_id}`
