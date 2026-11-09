# Remove a member

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/members/remove-member)

Takes a member's access away at once. The mail they sent and the API keys they created stay with the workspace. The owner cannot be removed (409 `owner_protected`). Admin only.

`DELETE /members/{member_id}`
