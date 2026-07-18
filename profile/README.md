# OpenRTMP

**Modern RTMP infrastructure for developers and stream operators.**

Build with a Rust RTMP/RTMPS and Enhanced RTMP protocol library, or deploy a private RTMP server with stream keys, REST API, live statistics, and a browser control panel.

[![Website](https://img.shields.io/badge/website-openrtmp.org-ff5c35)](https://openrtmp.org/)
[![Status](https://img.shields.io/badge/status-active%20alpha-red)](https://openrtmp.org/)
[![License](https://img.shields.io/badge/license-MIT-blue)](https://opensource.org/license/mit)

[Website](https://openrtmp.org/) · [Five-minute Docker quickstart](https://openrtmp.org/quickstart/) · [Guides](https://openrtmp.org/guides/) · [Documentation](https://openrtmp.org/docs/) · [Contributing](../CONTRIBUTING.md)

> **Project status:** OpenRTMP is active alpha software. It is suitable for development, evaluation, protocol work, and deployments that have been tested against their exact clients and recovery requirements. Pin versions and validate the complete workflow before critical production use.

## Choose your path

| I want to… | Start here |
|---|---|
| Build a custom RTMP server, client, relay, plugin, or gateway | [`librtmp2`](https://github.com/OpenRTMP/librtmp2) |
| Run a private RTMP/RTMPS endpoint with keys, API, and statistics | [`librtmp2-server`](https://github.com/OpenRTMP/librtmp2-server) |
| Manage streams and view live statistics in a browser | [`librtmp2-server-panel`](https://github.com/OpenRTMP/librtmp2-server-panel) |
| Evaluate the complete stack with Docker and OBS | [Five-minute quickstart](https://openrtmp.org/quickstart/) |
| Understand current protocol limitations | [`librtmp2` implementation status](https://github.com/OpenRTMP/librtmp2#implementation-status) |
| Compare OpenRTMP with nginx-rtmp | [OpenRTMP vs nginx-rtmp](https://openrtmp.org/guides/openrtmp-vs-nginx-rtmp/) |

## Try the complete stack

The standalone Compose stack pulls published server, panel, and Redis images. You don't need to clone or build `librtmp2` or `librtmp2-server`; clone the panel repository only to get its `compose.quickstart.yml`.

```bash
git clone https://github.com/OpenRTMP/librtmp2-server-panel.git
cd librtmp2-server-panel

# Generate the required API token, panel password, and session secret
# as shown in the full quickstart, then:
docker compose -f compose.quickstart.yml up -d
```

- Panel: `http://localhost:8000`
- API health: `http://localhost:8080/api/v1/health`
- RTMP: `rtmp://localhost:1935/live`

Follow the [complete quickstart](https://openrtmp.org/quickstart/) for copy-and-paste secret generation, OBS setup, health checks, troubleshooting, and the internet-facing deployment checklist.

## Projects

### [`librtmp2`](https://github.com/OpenRTMP/librtmp2)

A Rust protocol library for RTMP/RTMPS session handling, publish/play relay primitives, AMF, chunking, parser modules for Enhanced RTMP structures, and a C-compatible FFI.

Use it when your application owns authentication, storage, routing, transcoding, recording, or other media policy.

### [`librtmp2-server`](https://github.com/OpenRTMP/librtmp2-server)

A focused RTMP/RTMPS application layer built on `librtmp2`:

- SQLite-backed stream registry
- Per-stream publish, play, and statistics keys
- Bearer-authenticated REST API
- JSON statistics
- nginx-rtmp-compatible XML statistics
- Optional RTMPS listener alongside plaintext RTMP
- Published multi-architecture Docker images

The current server does **not** aim to provide nginx-rtmp feature parity for HLS, recording, `exec`, push relay, or every nginx directive.

### [`librtmp2-server-panel`](https://github.com/OpenRTMP/librtmp2-server-panel)

A Flask web UI for creating and deleting streams, copying publish/play/statistics URLs, and monitoring live bitrate, codec, resolution, frame rate, RTT, uptime, publishers, and players.

## Enhanced RTMP and codec support

OpenRTMP works on modern codec and Enhanced RTMP workflows, including HEVC, AV1, and Opus signaling/passthrough where supported by the complete sender/server/player chain.

Protocol parser support, opaque media relay, and fully integrated session negotiation are different levels of implementation. The [`librtmp2` implementation status](https://github.com/OpenRTMP/librtmp2#implementation-status) is the code-accurate source of truth for what is complete, partial, parser-only, or not yet wired into the default live path.

## Good fit today

- Private RTMP/RTMPS ingest and playback
- Custom Rust or FFI-based RTMP applications
- OBS and FFmpeg interoperability work
- Stream-key and statistics integrations
- nginx-compatible monitoring migrations
- Enhanced RTMP research and implementation
- Parser, fuzzing, and network-safety contributions

Consider a broader media platform when you need a turnkey viewer website, built-in HLS, recording, transcoding, push relay, or many non-RTMP protocols.

## Contributing

Contributions are welcome across protocol code, server behavior, the panel, documentation, interoperability tests, fuzzing, and deployment examples.

1. Read the organization-wide [contributing guide](../CONTRIBUTING.md).
2. Choose the repository that owns the behavior.
3. Search existing issues and discussions before opening a duplicate.
4. Start with an issue labeled `good first issue` or `help wanted` when available.
5. Include tests and exact reproduction steps for behavioral changes.

See the public [roadmap](../ROADMAP.md) for current priorities and the [support guide](../SUPPORT.md) for where to ask questions or report bugs.

## Security

Do not publish security-sensitive details in a public issue. Follow the organization [security policy](../SECURITY.md).

## License

OpenRTMP projects are released under the MIT License unless a repository states otherwise.
