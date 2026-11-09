# Release emails from the pacing queue

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/pacing/release-pacing-queue)

Releases up to 500 held emails immediately. `count_toward_limits` defaults to true; setting it to false means the sends do not consume rule budget, which makes the rule under-count — reserve it for genuinely exceptional releases. For mail that should never queue, set `pacing.skip` on the send instead.

`POST /pacing-queue/release`
