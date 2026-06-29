# OpenRTMP

Modern, open-source RTMP and Enhanced RTMP protocol stack for streaming applications.

[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
![Language](https://img.shields.io/badge/language-Rust-orange)
![Status](https://img.shields.io/badge/status-alpha-red)

---

## About

OpenRTMP provides a complete, production-ready implementation of RTMP (Real-Time Messaging Protocol) and its modern evolution, Enhanced RTMP (E-RTMP v1/v2). It's designed for developers and organizations building streaming solutions, media servers, broadcast tools, and OBS/FFmpeg integrations.

The project is split into focused, reusable components:

- **[librtmp2](https://github.com/OpenRTMP/librtmp2)** — Core protocol library (Rust, FFI-compatible)
- **[librtmp2-server](https://github.com/OpenRTMP/librtmp2-server)** — Ready-to-run media server (Rust)
- **[librtmp2-server-panel](https://github.com/OpenRTMP/librtmp2-server-panel)** — Web management panel
- **[.github](https://github.com/OpenRTMP/.github)** — Organization documentation and CI/CD

> **Note:** `librtmp2` and `librtmp2-server` are both Rust today, but they are separate codebases — `librtmp2-server` does not yet depend on the `librtmp2` crate for its RTMP listener. See [Architecture](#architecture) below.

---

## Projects

### 📦 [librtmp2](https://github.com/OpenRTMP/librtmp2)

A Rust library implementing the complete RTMP protocol stack — a 1:1 port of the original C `librtmp2`. Pure protocol logic with no media server, HTTP, or authentication policy built-in. Exposes both an idiomatic Rust API and an FFI-compatible `extern "C"` API (built as `cdylib`/`staticlib`/`lib`).

**Key Features:**
- ✅ Legacy RTMP handshake, chunking, and commands
- ✅ E-RTMP v1: ExVideo, FourCC codecs (HEVC, AV1, VP9), HDR metadata
- ✅ E-RTMP v2: Capability negotiation, reconnect, multitrack, ModEx
- ✅ AMF0/AMF3 encoding and decoding
- ✅ RTMPS (RTMP over TLS) via the optional `tls` Cargo feature (OpenSSL), enabled by default
- ✅ `extern "C"` FFI layer for consumption from C, Go, Python, PHP, and others
- ✅ Callback-based architecture for full control

**Perfect for:**
- Building custom RTMP servers
- OBS/FFmpeg plugins
- Broadcast tooling
- Protocol research and education

---

### 🎥 [librtmp2-server](https://github.com/OpenRTMP/librtmp2-server)

A media server written in **Rust** (axum + rusqlite) that owns everything around the RTMP protocol — config, persistence, HTTP/REST API, CLI, logging, and stream key generation.

**Key Features:**
- 💾 SQLite-backed persistence (streams, publishers, players, stats)
- 🔐 Key-based access control (`publish_key`, `play_key`, `stats_key`)
- 📊 JSON and Nginx-compatible XML stats endpoints
- 🔌 REST API for stream management (Bearer token auth)
- 🐳 Docker-ready with Alpine base
- ⚠️ **In progress:** the RTMP/E-RTMP wire protocol (the `librtmp2` crate) is not yet wired into the server's listener — see [`src/server.rs`](https://github.com/OpenRTMP/librtmp2-server/blob/main/src/server.rs)

**Perfect for:**
- Private streaming infrastructure (once protocol integration lands)
- Stream key/stats/API tooling around an RTMP deployment
- Contributors interested in finishing the `librtmp2` integration

---

### 🖥️ [librtmp2-server-panel](https://github.com/OpenRTMP/librtmp2-server-panel)

A lightweight Flask web panel for managing librtmp2-server. Create streams, monitor stats, and manage keys — all from a browser.

**Key Features:**
- 🌐 Web-based stream management (create, delete, monitor)
- 📊 Live stream statistics (bitrate, codec, viewership)
- 🔐 Key-based publish and play URL generation
- 🔒 CSRF protection, rate limiting, encrypted key storage
- 🐳 Docker-ready deployment
- 🔐 Bearer token authentication for API coordination
- 🍞 Bootstrap dark theme

**Perfect for:**
- Stream operators managing live events
- Production stream monitoring
- Multi-user stream management
- Integrating librtmp2-server into custom tooling

---

## Architecture

`librtmp2` and `librtmp2-server` are both Rust, but independent codebases today — the server does not yet depend on the `librtmp2` crate for its RTMP listener.

```text
┌─────────────────────┐
│  OBS / FFmpeg / App │
└──────────┬───────────┘
           │
           ▼
   ┌──────────────────┐      ┌─────────────────────────┐
   │  librtmp2 (Rust)  │      │  librtmp2-server (Rust)  │
   ├──────────────────┤      ├─────────────────────────┤
   │ Handshake         │      │ HTTP API (axum)          │
   │ Chunking          │      │ SQLite persistence       │
   │ AMF 0/3           │      │ Stream keys / stats      │
   │ Commands          │      │ RTMP listener: pending,  │
   │ E-RTMP v1/v2      │      │  blocked on librtmp2     │
   │ RTMPS (TLS)       │      │  crate integration       │
   │ extern "C" FFI    │      └─────────────────────────┘
   └──────────────────┘
           ▲
           │
   Embed directly into your own
   server / relay / plugin (Rust or FFI)
```

---

## Quick Start

### Using librtmp2 (Protocol Library, Rust)

```bash
git clone https://github.com/OpenRTMP/librtmp2.git
cd librtmp2
cargo build --release
cargo test
```

See the [librtmp2 repository](https://github.com/OpenRTMP/librtmp2) for crate docs and the `extern "C"` FFI surface in `src/lib.rs`.

### Using librtmp2-server (Media Server, Rust)

```bash
git clone https://github.com/OpenRTMP/librtmp2-server.git
cd librtmp2-server
cargo build --release
./target/release/librtmp2-server -c config.example.json
```

See [librtmp2-server README](https://github.com/OpenRTMP/librtmp2-server#build) for configuration and deployment. Note: RTMP ingest is not yet live — see the Architecture section above.

### Using librtmp2-server-panel (Web Panel)

```bash
git clone https://github.com/OpenRTMP/librtmp2-server-panel.git
cd librtmp2-server-panel
cp .env.example .env   # Edit with your settings
docker compose up -d   # Access at http://localhost:8000
```

See [librtmp2-server-panel README](https://github.com/OpenRTMP/librtmp2-server-panel#quick-start) for configuration.

---

## Use Cases

| Use Case | Solution |
|----------|----------|
| **OBS/FFmpeg Plugin** | Use `librtmp2` (Rust, FFI-compatible) to add RTMP ingest support from any language |
| **Private RTMP Relay** | Build on `librtmp2`; `librtmp2-server` is not yet protocol-capable |
| **Stream Key / Stats / API Tooling** | Use `librtmp2-server` for its HTTP API, SQLite persistence, and key management |
| **Broadcast Tool** | Use `librtmp2` for protocol handling, focus on your unique logic |
| **Protocol Research** | Study the reference implementation in `librtmp2` |

---

## Documentation

- **[librtmp2 source](https://github.com/OpenRTMP/librtmp2/tree/main/src)** — Module layout (`amf`, `chunk`, `ertmp`, `flv`, `handshake`, `message`, `server`, `session`, `transport`)
- **[librtmp2 FFI surface](https://github.com/OpenRTMP/librtmp2/blob/main/src/lib.rs)** — `extern "C"` exports for non-Rust consumers
- **[librtmp2-server Project Structure](https://github.com/OpenRTMP/librtmp2-server#project-structure)** — Rust module layout
- **[librtmp2-server Deployment](https://github.com/OpenRTMP/librtmp2-server#docker)** — Docker deployment

---

## Building & Testing

Both `librtmp2` and `librtmp2-server` are Rust crates built with a stable Rust toolchain.

```bash
# librtmp2
cargo build --release
cargo test

# librtmp2-server
cargo build --release
cargo test
```

`librtmp2`'s `tls` Cargo feature (OpenSSL-backed RTMPS) is enabled by default; build without it via `cargo build --no-default-features`. SQLite for `librtmp2-server` is vendored via rusqlite's `bundled` feature — no system SQLite3 needed.

---

## Contributing

We welcome bug reports, feature requests, and pull requests. Please open an issue first to discuss significant changes.

### Development

1. Fork the repository you want to contribute to
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Make your changes and run tests: `cargo test`
4. Commit with clear messages
5. Push to your fork and open a pull request

### Code Standards

- Follow existing idiomatic Rust style for the repository you're contributing to
- Add unit tests for new functionality
- Ensure all tests pass: `cargo test`

---

## Security

- Parser safety is paramount — all network-provided lengths are bounds-checked
- No buffer overflows, integer overflows, or invalid memory access
- See `SECURITY.md` for responsible disclosure

---

## License

All OpenRTMP projects are licensed under the **MIT License** — free to use, modify, and distribute, including for commercial purposes. See individual repositories for the LICENSE file.

---

## Support

- **Issues**: Report bugs and feature requests on the relevant GitHub repository
- **Discussions**: Use GitHub Discussions for questions and design conversations
- **Security**: See `SECURITY.md` in each repository for vulnerability reporting

---

## Roadmap

### librtmp2
- [x] RTMPS (RTMP over TLS) support
- [ ] End-to-end test suites for more edge cases
- [ ] Performance benchmarks and optimization

### librtmp2-server
- [ ] Wire the `librtmp2` crate into the server listener (live ingest/playback)
- [ ] RTMPS listener (depends on the above)
- [ ] Load balancing and clustering

---

## Related Projects

- **[FFmpeg RTMP Module](https://ffmpeg.org/)** — Audio/video transcoding
- **[OBS Studio](https://obsproject.com/)** — Open Broadcaster Software
- **[RTMP Spec](https://rtmp.veriskope.com/pdf/amf0-file-format-specification.pdf)** — Original Adobe specification
- **[RTMP Forum](https://github.com/veovera/enhance-rtmp)** — Enhanced RTMP specifications

---

## Acknowledgments

OpenRTMP is maintained by the community and inspired by the needs of modern streaming infrastructure. Built on years of RTMP protocol experience and battle-tested in production.

---

**Made with ❤️ for the open-source streaming community.**
