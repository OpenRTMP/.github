# AGENTS.md

This is the OpenRTMP organization meta repository. It holds the organization profile page (`profile/README.md` with images in `profile/assets/`, rendered on the GitHub organization homepage) and the default community health files that apply to every OpenRTMP repository without its own copy: `SECURITY.md`, `SUPPORT.md`, `CONTRIBUTING.md`, the pull request template, and the issue chooser in `.github/ISSUE_TEMPLATE/config.yml`. `ROADMAP.md` is organization documentation linked from the profile; GitHub does not inherit it into other repositories. There is no application, build step, service, or test here.

To preview the profile, view `profile/README.md` as Markdown; no tooling is required. Link other files in this repository from the profile with absolute `https://github.com/OpenRTMP/.github/blob/main/...` URLs so the links work on the organization page as well as in the repository view. `profile/assets/architecture.png` is a rendered image; edit it as an image, not as Mermaid (GitHub's Mermaid renderer clips its two-line labels).

The actual OpenRTMP products live in sibling repositories:
- `librtmp2` — Rust RTMP/E-RTMP protocol library
- `librtmp2-server` — Rust RTMP/E-RTMP media server (HTTP API + bundled SQLite)
- `librtmp2-server-panel` — Flask web UI for the server
- `packages` — native `librtmp2` packages and their build automation
- `openrtmp.org` — PHP marketing/docs website
- `community` — central issue forms and discussions for all components
