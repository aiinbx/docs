# Download prepared attachment text

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/attachments/retrieve-attachment-content)

Redirects to a short-lived signed URL for the attachment's prepared text — Markdown where the document has structure, plain text otherwise. Returns 404 `not_prepared` when the file type has no text to extract or preparation failed.

`GET /attachments/{attachment_id}/content`
