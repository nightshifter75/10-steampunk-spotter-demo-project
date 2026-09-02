# Steampunk Spotter - Cisco IOS Network Automation Demo

A deliberately mixed-quality Ansible project intended for demonstrating **Steampunk Spotter Scan** against Cisco IOS / IOS XE automation content.

> **Important:** files under `bad/` intentionally contain insecure, invalid, obsolete-looking, or poor-practice constructs. They are training inputs, not production examples.

## Lab topology

| Host | Role | Documentation IP |
|---|---|---|
| `rtr01` | Router | `192.0.2.11` |
| `sw01` | Access switch | `192.0.2.21` |
| `sw02` | Access switch | `192.0.2.22` |

The addresses come from documentation ranges and are not expected to be reachable.

## Project structure

```text
spotter-network-demo/
├── .spotter.yml
├── ansible.cfg
├── requirements.yml
├── requirements-legacy.yml
├── inventory/
│   ├── hosts.yml
│   └── group_vars/cisco_ios.yml
├── good/
│   ├── 001_interfaces.yml
│   ├── 002_vlans.yml
│   ├── 003_l2_interfaces.yml
│   ├── 004_l3_interfaces.yml
│   └── 005_acl.yml
├── bad/
│   ├── 101_legacy_and_redirected_modules.yml
│   ├── 102_wrong_parameters.yml
│   ├── 103_security_issues.yml
│   ├── 104_command_and_idempotency.yml
│   └── 105_everything_wrong.yml
└── docs/
    └── DEMO-CHEATSHEET.md
```

## What the GOOD playbooks demonstrate

The `good/` directory uses Cisco IOS resource modules to model desired state declaratively:

- `cisco.ios.ios_interfaces`
- `cisco.ios.ios_vlans`
- `cisco.ios.ios_l2_interfaces`
- `cisco.ios.ios_l3_interfaces`
- `cisco.ios.ios_acls`
- `cisco.ios.ios_acl_interfaces`

These files favor canonical FQCNs and `state: merged` so they are useful as a contrast with the deliberately imperative examples.

## What the BAD playbooks demonstrate

### 101 - Legacy / redirects / generic config
Uses redirected names such as `cisco.ios.interfaces` and pushes CLI text via `ios_config` where a resource module can express the intent more clearly.

### 102 - Wrong parameters
Contains an invented argument and a misspelled `commands` parameter. This is intended to exercise Spotter's knowledge of module interfaces.

### 103 - Security issues
Contains obvious hardcoded credentials and secret-like values, plus debug output that would expose them. **Never use this pattern in real code.**

### 104 - Command and idempotency anti-patterns
Uses `ios_command`/`ios_config` in ways that make for a useful discussion about state, idempotency and dedicated resource modules such as hostname/NTP resources.

### 105 - Everything wrong
A compact torture test combining redirects, raw CLI, invalid arguments, a hardcoded password, `with_items`, a compatibility-sensitive conditional, `shell`, `changed_when: true`, and play-level `ignore_errors`.

## Spotter scan suggestions

```bash
spotter scan --profile default good/
spotter scan --profile full bad/
spotter scan --profile security bad/103_security_issues.yml
```

For an upgrade-oriented demo, compare targets:

```bash
spotter scan --profile full --ansible-version 2.18 bad/105_everything_wrong.yml
spotter scan --profile full --ansible-version 2.20 bad/105_everything_wrong.yml
```

The project-level `.spotter.yml` defaults to Ansible 2.20.

## SBOM and CVE discussion

`requirements.yml` represents the modern dependency intent for the demo. `requirements-legacy.yml` deliberately pins much older collection versions so that you can discuss dependency age, compatibility and — where your Spotter edition and current knowledge base provide it — SBOM/CVE analysis.

**Do not claim that the legacy set has a particular CVE simply because it is old.** Use the actual Spotter report as the source of truth for CVE IDs, affected versions and severity.

## Why Cisco IOS

Cisco IOS is a convenient platform for demonstrating the difference between generic command/config automation and modern Ansible network **resource modules**, which model interfaces, VLANs, ACLs and other resources declaratively.

## Safety

The repository is designed primarily for static analysis. Before executing anything against real equipment:

- replace documentation addresses;
- use a real inventory and authenticated Ansible credential;
- remove all files under `bad/` from execution paths;
- review supported IOS/IOS XE versions and collection compatibility;
- test in a lab first.

See `docs/DEMO-CHEATSHEET.md` for the meeting sequence and talking points.
