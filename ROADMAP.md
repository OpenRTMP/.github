# OpenRTMP Roadmap

This roadmap communicates direction, not guaranteed dates or release commitments. Priorities may change based on interoperability findings, security work, maintainer capacity, and contributor interest.

OpenRTMP is currently **active alpha**. The immediate goal is not feature-count parity with every media server; it is a reliable, well-tested RTMP/RTMPS and Enhanced RTMP foundation with clear application-layer components.

## Current priorities

### 1. Protocol correctness and interoperability

- Expand OBS and FFmpeg publish/play test coverage
- Add regression tests for reconnect, late join, aggregate messages, initialization frames, and multiple players
- Continue code-accurate implementation-status documentation
- Improve compatibility evidence across legacy RTMP, RTMPS, and Enhanced RTMP workflows
- Keep parser and session-path claims clearly separated

### 2. Safety and resource controls

- Extend fuzz coverage for network-facing parsers and state transitions
- Harden connection, reassembly, cache, and relay-queue limits
- Improve malformed-input and slow-client tests
- Review FFI ownership, lifetime, and error-reporting behavior
- Document operational limits and expected failure modes

### 3. Server reliability

- Improve restart, database migration, token management, and recovery behavior
- Expand API and statistics contract tests
- Validate RTMP and RTMPS listeners under sustained connection churn
- Improve observability without leaking stream keys or private stream names
- Keep Docker images reproducible and multi-architecture

### 4. Operator experience

- Maintain a standalone five-minute Docker stack
- Improve panel setup, health reporting, copied URLs, and error messages
- Add screenshots and a safe public demo path when practical
- Publish deployment guides for reverse proxies, RTMPS, backups, upgrades, and monitoring
- Keep nginx-compatible statistics integrations documented

### 5. Contributor experience

- Maintain focused `good first issue` and `help wanted` tasks
- Provide repository ownership guidance and cross-repository merge order
- Add deterministic checks for code, docs, Docker, and website changes
- Document architecture decisions and compatibility expectations
- Make release notes useful to operators and library consumers

## Alpha exit criteria

Before describing the ecosystem as beta, the project should have:

- Stable, documented behavior for the supported legacy RTMP publish/play path
- Repeatable RTMPS interoperability evidence
- Clear Enhanced RTMP support boundaries with tested end-to-end paths
- Documented resource limits and failure behavior
- Upgrade and rollback guidance for server and panel deployments
- CI coverage for supported platforms and container architectures
- No known critical parser, authentication, or credential-handling issues
- A public compatibility matrix for tested senders and players

## Beta goals

During beta, priorities shift toward compatibility stability and operational confidence:

- Reduce breaking public API and configuration changes
- Introduce explicit deprecation and migration policies
- Expand long-running and load-test evidence
- Stabilize REST API and statistics contracts
- Improve release automation and signed/provenance-aware artifacts where practical
- Publish a defined support matrix and minimum supported toolchain policy

## 1.0 direction

A 1.0 release should mean that supported APIs and workflows have explicit compatibility guarantees. It does not require implementing every historical RTMP command or nginx-rtmp feature.

Likely 1.0 requirements include:

- Stable public Rust and C FFI surfaces for the documented use cases
- Stable server configuration, database, REST API, and statistics contracts
- A clearly bounded and tested protocol support matrix
- Documented security and release processes
- Supported upgrade paths across stable releases
- Production-oriented operational guidance based on real deployments

## Explicit non-goals for the current server

Unless the roadmap changes through design discussion, `librtmp2-server` is not trying to become an all-in-one media platform. Built-in HLS packaging, transcoding, recording, public viewer pages, and broad multi-protocol routing are better handled by dedicated components or other projects.

## Suggesting roadmap changes

Open a focused [community issue](https://github.com/OpenRTMP/community/issues/new/choose) or [community discussion](https://github.com/OpenRTMP/community/discussions) describing:

- The user or developer problem
- Why it belongs in OpenRTMP rather than an adjacent component
- Which repository owns the change
- Interoperability and compatibility impact
- A testable completion definition

Large changes should begin with a [design discussion](https://github.com/OpenRTMP/community/discussions) before implementation.
