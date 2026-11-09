# Update a member

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/members/update-member)

Changes what a member may do: `admin` manages the workspace, its members and its keys; `member` works with mail. It takes effect on their next request. The owner's role is not changed here — 409 `owner_protected` — the workspace changes hands by a transfer, in the console. Admin only.

`PATCH /members/{member_id}`
