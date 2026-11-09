# Update pacing spread

[View on docs.aiinbx.com](https://docs.aiinbx.com/api/endpoints/pacing/update-pacing-spread)

Sets how much randomness is added to send timing. At 0 a rate-limited stream goes out on a metronome; higher values vary the gaps, costing a little throughput in exchange for traffic that does not look machine-generated. Applies to limit rules.

`PATCH /pacing-rules/spread`
