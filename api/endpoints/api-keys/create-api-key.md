# Create an API key

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/api-keys/create-api-key)

Creates an organization-wide API key and returns its secret exactly once, in the `key` field. Store it before the response is discarded — it cannot be recovered. Choose the narrowest scope the caller needs: `read`, `sending`, or `full`. The key reaches the whole workspace, spaces included. Admin only.

`POST /api-keys`
