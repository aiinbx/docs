# Retrieve a thread

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/threads/retrieve-thread)

Returns a thread with its messages inline, oldest first. Messages paginate independently of the thread list via `message_limit` and `message_cursor`, so a long conversation can be walked without loading it whole.

`GET /threads/{thread_id}`
