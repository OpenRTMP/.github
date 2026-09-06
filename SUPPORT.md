# OpenRTMP Support

OpenRTMP is a community-maintained alpha project. There is no guaranteed response time or commercial support commitment.

## Where to ask

### Reproducible bug, feature request, or interoperability problem

Use the central [OpenRTMP community issue tracker](https://github.com/OpenRTMP/community/issues/new/choose) for every OpenRTMP component.

Choose the closest component in the issue form. You do not need to determine the exact source repository before reporting a problem; maintainers can triage it centrally.

| Component | Typical scope | Source repository |
|---|---|---|
| `librtmp2` | Protocol, RTMPS transport, client/server sessions, relay, parsers, Enhanced RTMP, C FFI | [`OpenRTMP/librtmp2`](https://github.com/OpenRTMP/librtmp2) |
| `librtmp2-server` | Server API, SQLite, keys, statistics, listener configuration, clustering, server image | [`OpenRTMP/librtmp2-server`](https://github.com/OpenRTMP/librtmp2-server) |
| `librtmp2-server-panel` | Panel UI, authentication, copied URLs, API client, live statistics, panel image | [`OpenRTMP/librtmp2-server-panel`](https://github.com/OpenRTMP/librtmp2-server-panel) |
| `packages` | Debian, Ubuntu, Alpine packages and package automation | [`OpenRTMP/packages`](https://github.com/OpenRTMP/packages) |
| `openrtmp.org` | Website, documentation presentation, quickstart, or guides | [`OpenRTMP/openrtmp.org`](https://github.com/OpenRTMP/openrtmp.org) |
| `organization/community` | Shared policy, community process, or cross-project topics | [`OpenRTMP/community`](https://github.com/OpenRTMP/community) |

Use the appropriate issue form and include exact versions, deployment method, reproduction steps, sanitized logs, and the complete publisher → server → player path when relevant.

### Usage question, setup help, idea, or design discussion

Use [OpenRTMP Community Discussions](https://github.com/OpenRTMP/community/discussions) when the topic is not yet an actionable issue, when you need setup help, or when you want feedback on architecture before implementing a large change.

### Security issue

Do not open a public issue or discussion. Follow [SECURITY.md](SECURITY.md).

## Before asking for help

1. Check the relevant README, documentation, and implementation-status table.
2. Search existing [community issues](https://github.com/OpenRTMP/community/issues) and [discussions](https://github.com/OpenRTMP/community/discussions).
3. Test the latest supported release or current default branch.
4. Reduce the setup to the smallest case that still fails.
5. Remove API tokens, stream keys, certificates, private hosts, and personal data.

## Information that speeds up diagnosis

- OpenRTMP component and version/commit
- Operating system and CPU architecture
- Docker image tag, package version, or native build command
- OBS, FFmpeg, player, browser, or integration versions
- Audio/video codecs and output settings
- RTMP or RTMPS URL shape with credentials removed
- Relevant environment variables with secrets replaced by `<redacted>`
- Logs from the first failure through disconnect/recovery
- Whether the problem occurs with one or multiple publishers/players
- Whether late join, reconnect, clustering, or server restart is involved

## Cross-repository work

Source-code pull requests stay in the repository that owns the implementation. When a change spans multiple repositories, use one central [community issue](https://github.com/OpenRTMP/community/issues/new/choose) to track the related pull requests and merge order.

## Scope

The current server does not provide built-in HLS, recording, transcoding, `exec`, push relay, a public viewer website, or broad non-RTMP protocol support. Requests that fundamentally require an all-in-one media platform may be redirected to another project or proposed as an external integration.

## Community conduct

Be patient and specific. Maintainers and contributors may request a smaller reproduction, additional logs, or an interoperability test before treating a report as actionable.
