# Steampunk Spotter Test Playbooks

A set of 10 Ansible playbooks for **Cisco IOS** devices (switches/routers), designed to be uploaded to spotter.steampunk.si to test its **Scan** functionality: CVE detection, deprecated/obsolete modules, and improper module usage.

## Playbook Map

| # | File | What It Tests |
|---|------|---------------|
| 1 | 01_interfaces_config.yml | Use of `ios_config` (raw commands) instead of dedicated modules (`ios_interfaces`/`ios_l2_interfaces`) → **wrong module usage** |
| 2 | 02_vlans_legacy.yml | **Deprecated** `ios_vlan` module (should use `ios_vlans`) → **obsolescence** |
| 3 | 03_l3_interfaces_legacy.yml | **Deprecated** `ios_l3_interface` module (should use `ios_l3_interfaces`) → **obsolescence** |
| 4 | 04_static_routes_legacy.yml | **Deprecated** `ios_static_route` module (should use `ios_static_routes`) → **obsolescence** |
| 5 | 05_acl_config.yml | Modern `ios_acls` module → **correct** comparison example |
| 6 | 06_ntp_logging.yml | NTP with correct `ios_ntp_global` module + logging via `ios_command` instead of dedicated `ios_logging_global` → **wrong module usage** |
| 7 | 07_bgp_config.yml | Modern `ios_bgp_global` module → **correct** example |
| 8 | 08_user_accounts.yml | **Deprecated** `ios_user` module + plaintext password in playbook → **obsolescence + security hygiene** |
| 9 | 09_facts_backup.yml | `ios_facts` with `gather_subset: all` (overly broad) → potential **optimization recommendation** |
| 10 | 10_full_declarative_modern.yml | Same VLAN/L2/L3 configuration as playbooks 2–3, but using **only modern resource modules** → "best practice" benchmark |

## Supporting Files

- `requirements.yml`: **Intentionally outdated** collection versions (cisco.ios 3.3.0, ansible.netcommon 5.1.0) to verify whether Spotter flags known CVEs or obsolete versions at the collection level.
- `inventory.ini`: Sample inventory (dummy IPs) to use if Spotter requires an inventory to link the scan with target hosts.

## How to Use Them in the Demo

1. Upload individual `.yml` files (or the entire folder, if Spotter supports uploading multiple files or a repository) into the platform's **Scan** section.
2. Expect playbooks 2, 3, 4, and 8 to trigger **deprecation/obsolescence** warnings.
3. Expect playbooks 1 and 6 to produce **best practice / wrong module usage** flags (using `ios_config`/`ios_command` instead of dedicated modules).
4. Expect playbook 8 to also flag the **plaintext password** issue.
5. Use playbooks 5, 7, and 10 as a "clean" baseline to demonstrate how a scan result without significant warnings should look.
6. If required, present `requirements.yml` to demonstrate CVE checks at the collection version level.

> [!NOTE] IPs, hostnames, and credentials are dummy data used solely for static analysis demonstration purposes; these playbooks are not intended to be run against real production devices.

# 💡 AAP Core Compatibility & Steampunk Spotter Demo Guide

Before running the commands, here is an important clarification based on the official **Red Hat Ansible Automation Platform (AAP)** lifecycle documentation:

> 📌 **Key Takeaway**: **AAP 2.5, 2.6, and 2.7** all ship with **`ansible-core 2.16`** as their default version. 
> 
> The true highlight of **AAP 2.7** is that **`ansible-core 2.20`** is available as an **Execution Environment (EE) stream** exclusively in AAP 2.7 (it is *not* the platform default). Note that RHEL 8 is not supported as a managed OS for Core 2.20; customers requiring RHEL 8 support on control/managed nodes must stick with the 2.16 default. Additionally, **`ansible-core 2.18`** is available and supported across the entire lifecycle of AAP 2.5, 2.6, and 2.7.

---

## 📊 Core Version Comparison Matrix

The most accurate and compelling comparison for your demo is not comparing defaults across versions, but rather highlighting the available Ansible Core streams:

| Context / Role | Core Version | Spotter Flag |
| :--- | :---: | :---: |
| **Default** across AAP 2.5 / 2.6 / 2.7 | `2.16` | `-a 2.16` |
| **Modern Option** available in 2.5 / 2.6 / 2.7 | `2.18` | `-a 2.18` |
| **Exclusive EE Stream** introduced in AAP 2.7 | `2.20` | `-a 2.20` |

---

## 🎯 Practical Demo Tips & Workflow

### 1. ⚙️ Use the `full` Profile
Rules related to module deprecations and removals across `ansible-core` versions are part of the upgrade profile checks. To execute these upgrade checks, always pass the `--profile full` flag:

```bash
spotter scan playbooks/*.yml -a 2.16 --profile full
spotter scan playbooks/*.yml -a 2.18 --profile full
spotter scan playbooks/*.yml -a 2.20 --profile full
