# Contributing to OpenRTMP

Thank you for helping improve OpenRTMP. Contributions are welcome in protocol code, server behavior, the panel, interoperability tests, fuzzing, documentation, deployment examples, and issue triage.

OpenRTMP is active alpha software. Clear scope, reproducible evidence, and tests are especially important while public APIs and protocol behavior are still evolving.

## Choose the correct repository

| Change | Repository |
|---|---|
| RTMP handshake, chunking, AMF, client/server sessions, relay behavior, RTMPS transport, Enhanced RTMP parsers, C FFI | [`OpenRTMP/librtmp2`](https://github.com/OpenRTMP/librtmp2) |
| Stream registry, authentication, SQLite, HTTP API, statistics, listener configuration, server Docker image | [`OpenRTMP/librtmp2-server`](https://github.com/OpenRTMP/librtmp2-server) |
| Browser UI, panel authentication, API client behavior, copied URLs, live statistics presentation, panel Docker image | [`OpenRTMP/librtmp2-server-panel`](https://github.com/OpenRTMP/librtmp2-server-panel) |
| Public website, guides, SEO, quickstart, docs presentation | [`OpenRTMP/openrtmp.org`](https://github.com/OpenRTMP/openrtmp.org) |
| Organization profile and shared community health files | [`OpenRTMP/.github`](https://github.com/OpenRTMP/.github) |

When a change spans multiple repositories, open one tracking issue that links the required pull requests and states the merge order.

## Before opening an issue

- Search existing open and closed issues.
- Check the relevant implementation-status section and current release notes.
- Reproduce the behavior with the latest supported release or the current default branch.
- Reduce the case to the smallest publisher/server/player combination that still fails.
- Remove stream keys, API tokens, certificates, private hostnames, and personal data.

General usage questions and design discussions belong in GitHub Discussions when they are not actionable bug reports.

## Good bug reports

Include:

- OpenRTMP component and exact version or commit
- Operating system, architecture, and deployment method
- OBS, FFmpeg, or player version
- Codec and relevant output settings
- RTMP or RTMPS transport and listener configuration
- Minimal reproduction steps
- Expected and actual behavior
- Sanitized logs
- Whether reconnect, late join, or multiple players are involved

A successful TCP connection or publish command does not prove end-to-end codec compatibility. Describe the complete publisher → server → player path.

## Development setup

### Rust projects

```bash
cargo fmt --all -- --check
cargo clippy --all-targets --all-features -- -D warnings
cargo test --all-features
```

Run repository-specific interoperability and fuzz tests when changing network parsers, state machines, media initialization, relay queues, or TLS behavior.

### Panel

Create a virtual environment, install the repository requirements, and run the documented test and lint commands. UI changes should include screenshots and should be checked at desktop and mobile widths.

### Website

```bash
php -S localhost:8090
find . -name '*.php' -print0 | xargs -0 -n1 php -l
```

Check canonical URLs, page titles, descriptions, internal links, code-block overflow, tables on mobile, `sitemap.xml`, and every new public path.

## Pull request expectations

Keep a pull request focused on one coherent change. The description should explain:

- What changed
- Why the change is needed
- User or developer impact
- Compatibility or migration impact
- Tests performed
- Known limitations or follow-up work

For protocol changes, distinguish clearly between:

- Parser/serializer support
- Default session-path integration
- Server application-layer behavior
- End-to-end interoperability evidence

Do not describe parser-only code as complete live-session support.

## Tests and compatibility

New behavior should include tests at the lowest useful layer. Bug fixes should include a regression test whenever practical.

Changes affecting public APIs, FFI layout, configuration variables, database schema, URL generation, Docker tags, or statistics formats must document compatibility impact.

## Documentation

Update the relevant README, implementation-status table, examples, and website guide in the same change or in linked pull requests. Avoid hard-coded “latest” version numbers on the website when crates.io or GitHub Releases can remain the source of truth.

## Commit and review hygiene

- Use descriptive commit messages.
- Do not mix formatting-only churn with behavior changes.
- Keep generated files and dependency updates intentional.
- Resolve review threads only after the requested change or explanation is present.
- Re-run relevant checks after rebasing or addressing review feedback.

## Security

Never include real credentials, private keys, or exploitable security details in public issues, pull requests, logs, or test fixtures. Follow [SECURITY.md](SECURITY.md) for private reporting.

## License

By contributing, you agree that your contribution is licensed under the license of the target repository, normally MIT.
