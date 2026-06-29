# OpenRTMP

Modern, open-source RTMP and Enhanced RTMP protocol stack for streaming applications.

[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
[![Languages](https://img.shields.io/badge/languages-C%20%7C%20Rust-blue)]()
[![Status](https://img.shields.io/badge/status-alpha-orange)]()

---

## About

OpenRTMP provides a complete, production-ready implementation of RTMP (Real-Time Messaging Protocol) and its modern evolution, Enhanced RTMP (E-RTMP v1/v2). It's designed for developers and organizations building streaming solutions, media servers, broadcast tools, and OBS/FFmpeg integrations.

The project is split into focused, reusable components:

- **[librtmp2](https://github.com/OpenRTMP/librtmp2)** — Core protocol library (C)
- **[librtmp2-server](https://github.com/OpenRTMP/librtmp2-server)** — Ready-to-run media server (Rust)
- **[librtmp2-server-panel](https://github.com/OpenRTMP/librtmp2-server-panel)** — Web management panel
- **[.github](https://github.com/OpenRTMP/.github)** — Organization documentation and CI/CD

> **Note:** `librtmp2` (C) and `librtmp2-server` (Rust) are separate codebases. `librtmp2-server` is building its own RTMP/E-RTMP protocol layer in Rust rather than binding the C library — see [Architecture](#architecture) below.

---

## Projects

### 📦 [librtmp2](https://github.com/OpenRTMP/librtmp2)

A modern C library implementing the complete RTMP protocol stack. Pure protocol logic with no media server, HTTP, or authentication policy built-in.

**Key Features:**
- ✅ Legacy RTMP handshake, chunking, and commands
- ✅ E-RTMP v1: ExVideo, FourCC codecs (HEVC, AV1, VP9), HDR metadata
- ✅ E-RTMP v2: Capability negotiation, reconnect, multitrack, ModEx
- ✅ AMF0/AMF3 encoding and decoding
- ✅ RTMPS (RTMP over TLS) via OpenSSL, optional at build time (`make TLS=0`)
- ✅ Callback-based architecture for full control
- ✅ Fuzz-tested parser safety
- ✅ FFI-compatible for Rust, Go, Python, PHP, and others

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
- ⚠️ **In progress:** the actual RTMP/E-RTMP wire protocol is a separate Rust crate that is not yet wired into the server's listener — see [`src/server.rs`](https://github.com/OpenRTMP/librtmp2-server/blob/main/src/server.rs)

**Perfect for:**
- Private streaming infrastructure (once protocol integration lands)
- Stream key/stats/API tooling around an RTMP deployment
- Contributors interested in finishing the Rust RTMP integration

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

`librtmp2` (C) and `librtmp2-server` (Rust) are independent codebases today — the server is not yet calling into the C library, and is instead growing its own Rust RTMP implementation.

```
┌─────────────────────┐
│  OBS / FFmpeg / App │
└──────────┬───────────┘
           │
           ▼
   ┌───────────────┐        ┌─────────────────────────┐
   │  librtmp2 (C) │        │  librtmp2-server (Rust)  │
   ├───────────────┤        ├─────────────────────────┤
   │ Handshake     │        │ HTTP API (axum)          │
   │ Chunking      │        │ SQLite persistence       │
   │ AMF 0/3       │        │ Stream keys / stats      │
   │ Commands      │        │ RTMP listener: pending,  │
   │ E-RTMP v1/v2  │        │  blocked on Rust RTMP    │
   │ RTMPS (TLS)   │        │  crate integration       │
   └───────────────┘        └─────────────────────────┘
           ▲
           │
   Embed directly into your own
   server / relay / plugin (FFI)
```

---

## Quick Start

### Using librtmp2 (Protocol Library, C)

```bash
git clone https://github.com/OpenRTMP/librtmp2.git
cd librtmp2
make debug                    # Build
make test                     # Run tests
make install                  # Install to /usr/local
```

See [librtmp2 README](https://github.com/OpenRTMP/librtmp2#build) for examples.

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
| **OBS/FFmpeg Plugin** | Use `librtmp2` (C) to add RTMP ingest support |
| **Private RTMP Relay** | Build on `librtmp2` (C); `librtmp2-server` (Rust) is not yet protocol-capable |
| **Stream Key / Stats / API Tooling** | Use `librtmp2-server` for its HTTP API, SQLite persistence, and key management |
| **Broadcast Tool** | Use `librtmp2` for protocol handling, focus on your unique logic |
| **Protocol Research** | Study the reference implementation in `librtmp2` |

---

## Documentation

- **[librtmp2 API](https://github.com/OpenRTMP/librtmp2/blob/main/include/librtmp2/)** — Public header files with inline docs
- **[librtmp2 CLAUDE.md](https://github.com/OpenRTMP/librtmp2/blob/main/CLAUDE.md)** — Build commands, architecture notes, design rules
- **[librtmp2-server Project Structure](https://github.com/OpenRTMP/librtmp2-server#project-structure)** — Rust module layout
- **[librtmp2-server Deployment](https://github.com/OpenRTMP/librtmp2-server#docker)** — Docker deployment

---

## Building & Testing

### librtmp2 (C)

Requirements: `gcc` or `clang`, `make`, `pthread`.

```bash
make debug         # Debug build with symbols
make release        # Optimized release build
make test           # Build and run all tests
make asan           # AddressSanitizer (memory safety)
make ubsan          # UndefinedBehaviorSanitizer
make fuzz           # LibFuzzer harnesses (clang only)
make TLS=0          # Build without OpenSSL / RTMPS
```

Supported platforms: Linux (x86_64, ARM64), macOS, Windows (WSL2).

### librtmp2-server (Rust)

Requirements: Rust stable toolchain (SQLite is vendored via rusqlite's `bundled` feature).

```bash
cargo build --release
cargo test
```

---

## Contributing

We welcome bug reports, feature requests, and pull requests. Please open an issue first to discuss significant changes.

### Development

1. Fork the repository you want to contribute to
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Make your changes and run tests (`make test` for C projects, `cargo test` for Rust)
4. Commit with clear messages
5. Push to your fork and open a pull request

### Code Standards

- Follow existing code style per repository (POSIX C for `librtmp2`, idiomatic Rust for `librtmp2-server`)
- Add unit tests for new functionality
- Ensure all tests pass before opening a PR
- Run sanitizers in CI for `librtmp2`: `make asan && make ubsan`

---

## Security

- Parser safety is paramount — all network-provided lengths are bounds-checked
- Fuzz test harnesses in `librtmp2/tests/fuzz/` ensure robustness
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

### librtmp2 (C)
- [x] RTMPS (RTMP over TLS) support
- [ ] End-to-end test suites for more edge cases
- [ ] Performance benchmarks and optimization

### librtmp2-server (Rust)
- [ ] Wire the Rust RTMP/E-RTMP protocol crate into the server listener (live ingest/playback)
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
