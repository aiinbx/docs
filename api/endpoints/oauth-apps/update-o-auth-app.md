# Update an OAuth app

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/oauth-apps/update-o-auth-app)

Updates an OAuth app's credentials, branding or return URLs. At least one field is required. A rotated secret replaces the old one immediately, so update the provider first. `return_urls` replaces the whole list.

`PATCH /oauth-apps/{app_id}`
