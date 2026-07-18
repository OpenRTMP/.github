# Security Policy

## Supported versions

OpenRTMP is active alpha software. Security fixes are generally made on the default branch and included in the next release. Older alpha releases may not receive backports.

For deployments, use a tested pinned release and monitor the affected repository for security advisories and new releases.

## Reporting a vulnerability

Please do **not** disclose suspected vulnerabilities in public issues, discussions, pull requests, logs, or example configurations.

Preferred reporting path:

1. Open the affected repository on GitHub.
2. Select **Security**.
3. Choose **Report a vulnerability** to create a private security advisory.

If private vulnerability reporting is not available for the affected repository, email `info@openrtmp.org` with the subject `SECURITY: <short description>` and avoid sending real production credentials or private keys.

## Include

- Affected repository, version, tag, or commit
- Vulnerability class and affected component
- Minimal reproduction or proof of concept
- Required attacker access and preconditions
- Expected impact
- Whether the issue is remotely exploitable
- Suggested mitigation, when known
- Whether the report is subject to a disclosure deadline

For network-protocol issues, identify whether the input reaches handshake, chunking, AMF, media parsing, session state, TLS, HTTP, authentication, database, or FFI code.

## Response process

Maintainers will attempt to:

1. Acknowledge the report.
2. Reproduce and assess severity.
3. Coordinate a fix and regression test.
4. Prepare release and mitigation guidance.
5. Publish an advisory when users need to take action.

Response times are not guaranteed because OpenRTMP is community maintained. Please allow reasonable coordination time before public disclosure unless immediate disclosure is necessary to protect users.

## Security-sensitive data

Never include:

- API bearer tokens
- Publish, play, or statistics keys
- TLS private keys
- Session secrets or passwords
- Private hostnames or customer data
- Unredacted production databases

Use generated test credentials and minimal isolated environments.

## Scope examples

Reports may include:

- Memory-safety, integer, bounds, or parser-state flaws
- Authentication or authorization bypass
- Credential disclosure
- TLS validation or key-handling problems
- Denial of service through malformed or resource-exhausting input
- Unsafe FFI ownership or lifetime behavior
- CSRF, session, rate-limit, or encryption weaknesses in the panel
- SQL or API access-control vulnerabilities

General hardening suggestions, unsupported deployment configurations, and missing non-security features should use normal issues or discussions.
