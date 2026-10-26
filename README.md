# Cyber Project

A hands-on IT support, systems administration, and security lab portfolio. This repository records completed configuration separately from planned work so each project reflects its actual state.

## Current status

| Area | Status | Scope |
| --- | --- | --- |
| [Enterprise IT Support](enterprise-it-support/) | In progress | Windows Server 2025 lab setup and AD DS role installation |
| [Linux Administration](linux-administration/) | Planned | Linux administration exercises and documentation |
| [SOC / SIEM Lab](soc-siem-lab/) | Planned | Security monitoring and investigation exercises |

## Verified lab setup

The enterprise IT support lab currently has a Windows Server 2025 Standard Evaluation virtual machine with Desktop Experience in VirtualBox. The server is named **DC01**. Its NAT adapter receives **10.0.2.15** through DHCP, and its internal **Ethernet 2** adapter is configured as **192.168.100.10/24**. The **Active Directory Domain Services (AD DS)** server role is installed.

**DC01 has not yet been promoted to a domain controller.** DNS configuration and domain controller promotion are planned. A Windows 11 client, Linux administration work, and SOC/SIEM work are also planned; no completion is claimed for those items.

## Repository layout

- [`enterprise-it-support/`](enterprise-it-support/) — Windows Server lab notes and future evidence.
  - [`docs/`](enterprise-it-support/docs/) — configuration notes and procedures.
  - [`screenshots/`](enterprise-it-support/screenshots/) — future redacted screenshots.
  - [`diagrams/`](enterprise-it-support/diagrams/) — future architecture diagrams.
  - [`powershell/`](enterprise-it-support/powershell/) — future PowerShell scripts.
- [`linux-administration/`](linux-administration/) — planned Linux work.
- [`soc-siem-lab/`](soc-siem-lab/) — planned SOC/SIEM work.

## Documentation standard

Only completed, verified steps are recorded as completed. Planned steps remain labeled as planned until tested and documented. Do not commit credentials, private keys, tokens, or sensitive lab data. Review screenshots and command output before publishing them.
