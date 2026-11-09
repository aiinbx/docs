# List emails

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/emails/list-emails)

Lists emails in the workspace, newest first. Filter by `direction`, delivery `status`, a single `thread_id`, or a literal `query`: every word in it must appear in the subject, the sender, a recipient or the Message-ID, in any order and case. The query is a literal filter; the API does not provide semantic email search. To sync, walk forward instead: `order=asc` with `after` set to the `created_at` of the last email you have lists only what is new, oldest first.

`GET /emails`
