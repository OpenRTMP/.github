# OpenRTMP

Modern, open-source RTMP and Enhanced RTMP protocol stack for streaming applications.

[![License](https://img.shields.io/badge/license-MIT%2FISC-blue)](LICENSE)
[![Language](https://img.shields.io/badge/language-C-blue)]()
[![Status](https://img.shields.io/badge/status-alpha-orange)]()

---

## About

OpenRTMP provides a complete, production-ready implementation of RTMP (Real-Time Messaging Protocol) and its modern evolution, Enhanced RTMP (E-RTMP v1/v2). It's designed for developers and organizations building streaming solutions, media servers, broadcast tools, and OBS/FFmpeg integrations.

The project is split into focused, reusable components:

- **[librtmp2](https://github.com/OpenRTMP/librtmp2)** — Core protocol library
- **[librtmp2-server](https://github.com/OpenRTMP/librtmp2-server)** — Ready-to-run media server
- **[.github](https://github.com/OpenRTMP/.github)** — Organization documentation and CI/CD

---

## Projects

### 📦 [librtmp2](https://github.com/OpenRTMP/librtmp2)

A modern C library implementing the complete RTMP protocol stack. Pure protocol logic with no media server, HTTP, or authentication policy built-in.

**Key Features:**
- ✅ Legacy RTMP handshake, chunking, and commands
- ✅ E-RTMP v1: ExVideo, FourCC codecs (HEVC, AV1, VP9), HDR metadata
- ✅ E-RTMP v2: Capability negotiation, reconnect, multitrack, ModEx
- ✅ AMF0/AMF3 encoding and decoding
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

A lightweight, production-ready RTMP/E-RTMP media server built on librtmp2. Handles ingest, playback, and stats with minimal overhead.

**Key Features:**
- 🎬 RTMP and E-RTMP v1/v2 ingest and playback
- 💾 SQLite-backed persistence (streams, publishers, players, stats)
- 🔐 Key-based access control (`publish_key`, `play_key`, `stats_key`)
- 📊 JSON and Nginx-compatible XML stats endpoints
- 🔌 REST API for stream management
- 🐳 Docker-ready with Alpine base
- ⚡ Low resource footprint

**Perfect for:**
- Private streaming infrastructure
- Live streaming platforms
- Broadcast relay
- Multi-protocol ingestion

---

## Architecture

```
┌─────────────────────┐
│  OBS / FFmpeg /App  │
└──────────────┬──────┘
               │
               ▼
        ┌──────────────┐
        │  librtmp2    │  ← Pure protocol
        ├──────────────┤
        │ Handshake    │
        │ Chunking     │
        │ AMF 0/3      │
        │ Commands     │
        │ E-RTMP v1/v2 │
        └──────────────┘
               ▲
               │
    ┌──────────┴──────────┐
    │                     │
┌───┴─────────┐  ┌────────┴──────────┐
│  librtmp2   │  │ Custom App        │
│  -server    │  │ (Relay, Plugin,   │
│             │  │  etc.)            │
└─────────────┘  └──────────────────┘
```

---

## Quick Start

### Using librtmp2 (Protocol Library)

```bash
git clone https://github.com/OpenRTMP/librtmp2.git
cd librtmp2
make debug                    # Build
make test                     # Run tests
make install                  # Install to /usr/local
```

See [librtmp2 README](https://github.com/OpenRTMP/librtmp2#quick-start) for examples.

### Using librtmp2-server (Media Server)

```bash
git clone https://github.com/OpenRTMP/librtmp2-server.git
cd librtmp2-server
make debug                    # Build
./build/server               # Run server (listens on :1935 and :8080)
```

See [librtmp2-server README](https://github.com/OpenRTMP/librtmp2-server#quick-start) for deployment.

---

## Use Cases

| Use Case | Solution |
|----------|----------|
| **OBS/FFmpeg Plugin** | Use `librtmp2` to add RTMP ingest support |
| **Private RTMP Relay** | Deploy `librtmp2-server` or use `librtmp2` to build custom relay |
| **Live Streaming Platform** | Start with `librtmp2-server` API, extend as needed |
| **Broadcast Tool** | Use `librtmp2` for protocol handling, focus on your unique logic |
| **Protocol Research** | Study the reference implementation in `librtmp2` |

---

## Documentation

- **[librtmp2 API](https://github.com/OpenRTMP/librtmp2/blob/main/include/librtmp2/)** — Public header files with inline docs
- **[librtmp2 CLAUDE.md](https://github.com/OpenRTMP/librtmp2/blob/main/CLAUDE.md)** — Build commands, architecture notes, design rules
- **[librtmp2-server Deployment](https://github.com/OpenRTMP/librtmp2-server#deployment)** — Docker, systemd, reverse proxy

---

## Building & Testing

### Requirements

- `gcc` or `clang` (Linux, macOS, or WSL)
- `make`
- `pthread` (usually included)

### Supported Platforms

- Linux (x86_64, ARM64)
- macOS
- Windows (WSL2)

### Compile Options

```bash
# librtmp2
make debug         # Debug build with symbols
make release       # Optimized release build
make test          # Build and run all tests
make asan          # AddressSanitizer (memory safety)
make ubsan         # UndefinedBehaviorSanitizer
make fuzz          # LibFuzzer harnesses (clang only)

# librtmp2-server
make debug         # Debug build
make release       # Release build
make test          # Integration tests
```

---

## Contributing

We welcome bug reports, feature requests, and pull requests. Please open an issue first to discuss significant changes.

### Development

1. Fork the repository you want to contribute to
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Make your changes and run tests: `make test`
4. Commit with clear messages
5. Push to your fork and open a pull request

### Code Standards

- Follow existing code style (POSIX C with clarity over cleverness)
- Add unit tests for new functionality
- Ensure all tests pass: `make test`
- Run sanitizers in CI: `make asan && make ubsan`

---

## Security

- Parser safety is paramount — all network-provided lengths are bounds-checked
- Fuzz test harnesses in `tests/fuzz/` ensure robustness
- No buffer overflows, integer overflows, or invalid memory access
- See `SECURITY.md` for responsible disclosure

---

## License

All OpenRTMP projects are dual-licensed:

- **MIT License** — Most permissive, suitable for commercial use
- **ISC License** — Alternative open-source license

Pick the one that works best for you. See individual repositories for LICENSE file.

---

## Support

- **Issues**: Report bugs and feature requests on the relevant GitHub repository
- **Discussions**: Use GitHub Discussions for questions and design conversations
- **Security**: See `SECURITY.md` in each repository for vulnerability reporting

---

## Roadmap

### librtmp2
- [ ] RTMPS (RTMP over TLS) support
- [ ] End-to-end test suites for more edge cases
- [ ] Performance benchmarks and optimization

### librtmp2-server
- [ ] RTMPS (RTMP over TLS) listener
- [ ] Load balancing and clustering
- [ ] Grafana/Prometheus stats export

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
