# Invite someone

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/members/create-invitation)

Mails an invitation to join the workspace, with a link that is good for seven days. Inviting an address that already has one waiting replaces it once the new mail has gone — the earlier link then stops working — which is also how an invitation is sent again. An address that is already a member fails with 409 `already_member`. When the mail cannot go, nothing is created and any earlier invitation keeps working: the address has been invited too often today, or the inviter has sent too many, fails with 429 `invitation_not_sent` — try again later; an address that asked not to hear from us fails with 400 `recipients_suppressed`, which will not change on retrying. Invitations sent with an API key are in the name of the person who created the key, or the owner once they are gone. Admin only.

`POST /invitations`
