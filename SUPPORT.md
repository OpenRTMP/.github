# OpenRTMP Support

OpenRTMP is a community-maintained alpha project. There is no guaranteed response time or commercial support commitment.

## Where to ask

### Reproducible bug

Open an issue in the repository that owns the behavior:

- Protocol, RTMPS transport, client/server session, relay, parser, or FFI: [`librtmp2`](https://github.com/OpenRTMP/librtmp2/issues)
- Server API, SQLite, keys, statistics, listener configuration, or server image: [`librtmp2-server`](https://github.com/OpenRTMP/librtmp2-server/issues)
- Panel UI, authentication, copied URLs, API client, or panel image: [`librtmp2-server-panel`](https://github.com/OpenRTMP/librtmp2-server-panel/issues)
- Website, quickstart, or guide: [`openrtmp.org`](https://github.com/OpenRTMP/openrtmp.org/issues)

Use the bug-report template and include exact versions, deployment method, reproduction steps, sanitized logs, and the complete publisher → server → player path.

### Usage question or design discussion

Use [GitHub Discussions](https://github.com/OpenRTMP/librtmp2/discussions) when the question is not yet a reproducible bug or when you want feedback on architecture before implementing a large change.

### Security issue

Do not open a public issue. Follow [SECURITY.md](SECURITY.md).

## Before asking for help

1. Check the relevant README and implementation-status table.
2. Search open and closed issues.
3. Test the latest supported release or current default branch.
4. Reduce the setup to the smallest case that still fails.
5. Remove API tokens, stream keys, certificates, private hosts, and personal data.

## Information that speeds up diagnosis

- Repository and version/commit
- Operating system and CPU architecture
- Docker image tag or native build command
- OBS, FFmpeg, and player versions
- Audio/video codecs and output settings
- RTMP or RTMPS URL shape with credentials removed
- Relevant environment variables with secrets replaced by `<redacted>`
- Logs from the first failure through disconnect/recovery
- Whether the problem occurs with one or multiple publishers/players
- Whether late join, reconnect, or server restart is involved

## Scope

The current server does not provide built-in HLS, recording, transcoding, `exec`, push relay, a public viewer website, or broad non-RTMP protocol support. Requests that fundamentally require an all-in-one media platform may be redirected to another project or proposed as an external integration.

## Community conduct

Be patient and specific. Maintainers and contributors may request a smaller reproduction, additional logs, or an interoperability test before treating a report as actionable.
