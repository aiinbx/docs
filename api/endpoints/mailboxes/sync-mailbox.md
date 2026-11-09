# Queue a mailbox sync

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/mailboxes/sync-mailbox)

Queues a catch-up sync for a mailbox. Mail normally arrives on its own through provider push notifications, so this is a repair tool rather than a way to poll. Messages found surface as ordinary `email.received` events.

`POST /mailboxes/{mailbox_id}/sync`
