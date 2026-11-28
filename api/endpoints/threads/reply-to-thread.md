# Reply in a thread

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/threads/reply-to-thread)

Replies within a thread. The sender, recipients, subject, and the `In-Reply-To` and `References` headers are inferred from the conversation, as a mail client infers them from the email answered: `email_id`, or the thread's latest. The reply follows that email's Reply-To; one that names only your own addresses is refused with 400 `no_recipients` rather than guessed around. Only the content is required — set `reply_all` or the individual fields to override what is inferred. Suppression and pacing behave exactly as on a send; an `Idempotency-Key` is bound to the request, so a retry replays the reply even after the thread has moved on. So does the plan: in a live workspace 402 `plan_required` without one, 402 `allowance_reached` once its emails are used up; a sandbox needs none, and counts each as one of its emails for the day, past them 429 `sandbox_limit_reached`.

`POST /threads/{thread_id}/reply`
