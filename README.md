# Cyber Project

A hands-on IT support, systems administration, and security lab portfolio. This repository records completed configuration separately from planned work so each project reflects its actual state.

## Current status

| Area | Status | Scope |
| --- | --- | --- |
| [Enterprise IT Support](enterprise-it-support/) | In progress | Windows Server 2025 lab; first domain controller promoted; DNS and client validation pending |
| [Linux Administration](linux-administration/) | Planned | Linux administration exercises and documentation |
| [SOC / SIEM Lab](soc-siem-lab/) | Planned | Security monitoring and investigation exercises |

## Verified lab setup

VirtualBox runs **DC01**, a Windows Server 2025 Standard Evaluation (Desktop Experience) VM with **4 GB RAM**, **2 CPUs**, and a **60 GB dynamic VDI**. Its NAT adapter has **10.0.2.15**; internal **Ethernet 2** has **192.168.100.10/24**. The AD DS role is installed, and DC01 was promoted into the **corp.example.test** forest. The user confirmed domain sign-in and running NTDS, DNS, and Netlogon services.

The exact AD domain/controller outputs, DNS records, multihomed DNS behavior, forwarding, and Windows 11 domain join remain to be verified. See the [dated progress log](enterprise-it-support/PROGRESS.md).

## Repository layout

- [`enterprise-it-support/`](enterprise-it-support/) — Windows Server lab notes and future evidence.
  - [`PROGRESS.md`](enterprise-it-support/PROGRESS.md) — dated checkpoints and next verification commands.
  - [`docs/`](enterprise-it-support/docs/) — configuration notes and procedures.
  - [`screenshots/`](enterprise-it-support/screenshots/) — future redacted screenshots.
  - [`diagrams/`](enterprise-it-support/diagrams/) — future architecture diagrams.
  - [`powershell/`](enterprise-it-support/powershell/) — future PowerShell scripts.
- [`linux-administration/`](linux-administration/) — planned Linux work.
- [`soc-siem-lab/`](soc-siem-lab/) — planned SOC/SIEM work.

## Documentation standard

Only completed, verified steps are recorded as completed. Planned steps remain labeled as planned until tested and documented. Do not commit credentials, private keys, tokens, or sensitive lab data. Review screenshots and command output before publishing them.
