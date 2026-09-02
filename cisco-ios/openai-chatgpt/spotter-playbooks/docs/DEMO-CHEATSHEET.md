# Spotter Network Demo - Meeting Cheat Sheet

## Suggested demo flow

1. Scan `good/` with the default profile.
2. Scan `bad/102_wrong_parameters.yml` and discuss module argument validation.
3. Scan `bad/103_security_issues.yml` with the security profile.
4. Scan `bad/105_everything_wrong.yml` with the full profile.
5. Compare target Ansible versions (for example 2.18 vs 2.20) to demonstrate upgrade guidance.
6. If your Spotter subscription exposes SBOM/CVE, inspect `cisco.ios` and `ansible.netcommon` from the project/collection report.
7. Compare `requirements.yml` with `requirements-legacy.yml`. The legacy file is intentionally old; do not claim a specific CVE unless Spotter actually reports one.

## Useful CLI commands

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install steampunk-spotter

spotter scan --profile default good/
spotter scan --profile security bad/103_security_issues.yml
spotter scan --profile full bad/
spotter scan --profile full --ansible-version 2.18 bad/105_everything_wrong.yml
spotter scan --profile full --ansible-version 2.20 bad/105_everything_wrong.yml
```

## Talking points

- Resource modules describe intended network state and are usually preferable to pushing CLI strings when a suitable resource module exists.
- Canonical FQCNs make intent and dependencies explicit.
- Module argument validation catches typos and parameters that no longer exist.
- Upgrade scans can expose behavior that changes across ansible-core releases.
- Security scanning is distinct from ordinary linting: secrets, risky constructs, collection dependencies, SBOM and CVE data can all be part of the conversation.
- A CVE finding depends on the exact collection/dependency version and Spotter knowledge base. Never pre-announce a CVE that the scan did not actually return.

## Expected findings - deliberately non-deterministic

Spotter evolves frequently, so exact event codes and severities may change. The files are designed to create categories of findings rather than guarantee a fixed screenshot.

- `101`: redirected/non-canonical modules; imperative CLI vs resource-module discussion.
- `102`: unknown/misspelled module parameters.
- `103`: hardcoded secrets and insecure credential handling.
- `104`: idempotency and generic command/config usage where dedicated resource modules exist.
- `105`: redirects, raw CLI, unknown parameters, hardcoded secret, `with_items`, conditional compatibility, shell usage and broad `ignore_errors`.
