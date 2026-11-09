# Delete a suppression

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/suppressions/delete-suppression)

Removes a suppression entry, allowing sends to that address again. Removing a `bounce` or `complaint` entry means mailing an address that already rejected you or reported you as spam — do it only with specific evidence the address was fixed. `provider_suppressed` says whether SES itself still refuses the address, which removing the entry does not change.

`DELETE /suppressions/{suppression_id}`
