# Simulate an inbound email

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/sandbox/simulate-inbound-email)

Writes an email into a sandbox as if it came from outside. It is composed into MIME and goes through everything mail from outside goes through: parsing, classification, threading, attachment preparation, extraction and the `email.received` webhook. `headers` and `verdicts` set what the sender's mail system and the receiving checks would, to try an automatic reply, a mailing list or spam. Every `to` has to be an address of the workspace, on one of its domains, or the request fails with 422 `recipient_unknown`. As with real mail, a copy is filed in each space its recipients are in; the response is the email filed for the first `to`. A bounce report is not filed as an email: it is handled as a real one is, and the request fails with 400 `invalid_request`; to try a bounce, send to `bounce@simulator.aiinbx.com`. Each copy filed counts as one of the sandbox's emails for the day, and past them the request fails with 429 `sandbox_limit_reached`. On a live workspace it fails with 403 `sandbox_only`.

`POST /sandbox/emails`
