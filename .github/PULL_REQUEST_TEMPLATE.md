## What changed

<!-- Summarize the externally visible behavior and the repository/component affected. -->

## Why

<!-- Explain the user/developer problem, root cause, or maintenance goal. -->

## Scope

- [ ] Protocol parser/serializer
- [ ] Default client/server session path
- [ ] Server application layer, API, database, or statistics
- [ ] Panel/UI
- [ ] Docker/deployment
- [ ] Documentation/website
- [ ] Cross-repository change with linked PRs

## Compatibility impact

<!-- Cover public Rust APIs, C FFI, configuration, database, URLs, statistics formats, Docker tags, and OBS/FFmpeg/player interoperability when relevant. Write "None" only after checking. -->

## Validation

<!-- List exact commands and end-to-end scenarios. -->

- [ ] Formatting/lint checks pass
- [ ] Unit and integration tests pass
- [ ] A regression test was added for a bug fix, where practical
- [ ] Publisher → server → player behavior was tested when relevant
- [ ] Reconnect and late join were considered when relevant
- [ ] RTMP and RTMPS were both considered when relevant
- [ ] Documentation and implementation-status tables were updated
- [ ] No credentials, private keys, stream keys, tokens, or private data are present

## Evidence

<!-- Include sanitized logs, screenshots for UI work, benchmark data, interoperability versions, or before/after behavior. -->

## Known limitations and follow-up

<!-- State what this PR intentionally does not implement. Distinguish parser-only work from live-session integration. -->
