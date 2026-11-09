# Inspect attachment metadata

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/attachments/inspect-attachment)

Returns the attachment's media type, size, and filename disposition in response headers without transferring the file — the cheap way to check a size before committing to a download.

`HEAD /attachments/{attachment_id}`
