# Update the workspace

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/workspace/update-workspace)

Renames the workspace, changes its tracking defaults or switches `thread_across_addresses` for mail outside spaces; only the fields given change. A new name brings a new slug, which moves the console URLs with it (the provided domain keeps its name), so a workspace can be renamed once a month — sooner fails with 409 `rename_cooldown`. Tracking defaults apply to domains added from now on; each existing domain keeps its own. Admin only.

`PATCH /workspace`
