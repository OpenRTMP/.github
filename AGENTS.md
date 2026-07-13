# AGENTS.md

## Cursor Cloud specific instructions

This is the OpenRTMP org meta repository. It contains only the org profile page (`profile/README.md`, rendered on the GitHub organization homepage) — there is no application, build step, service, or test here.

To preview the profile, view `profile/README.md` as Markdown; no tooling is required.

The actual OpenRTMP products live in sibling repositories, each with its own `AGENTS.md`:
- `librtmp2` — Rust RTMP/E-RTMP protocol library
- `librtmp2-server` — Rust RTMP/E-RTMP media server (HTTP API + bundled SQLite)
- `librtmp2-server-panel` — Flask web UI for the server
- `openrtmp.org` — PHP marketing/docs website
