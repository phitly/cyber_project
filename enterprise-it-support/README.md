# Enterprise IT Support

Windows Server and client administration lab. See the [progress log](PROGRESS.md) for dated checkpoints, verification commands, and next steps.

## Status through 2026-10-26

- **Verified:** DC01 runs Windows Server 2025 Standard Evaluation (Desktop Experience) in VirtualBox. NAT Ethernet has 10.0.2.15; internal Ethernet 2 has 192.168.100.10/24. The AD DS role is installed; DC01 was promoted into the corp.example.test forest, restarted, and signed in to the domain. The user confirmed NTDS, DNS, and Netlogon are running.
- **Verified on DC01:** domain name and NetBIOS name, DC01's internal IPv4 address and Global Catalog status, running NTDS/DNS/Netlogon, AD locator SRV record, internal-only DC01 host A record after cleanup, and external DNS resolution. See the progress log for commands and observed results.
- **Pending verification:** confirm the unwanted NAT A record does not return after a later registration cycle; capture remaining AD properties if needed. The configured IPv6 forwarder and root-hint fallback were observed, but the path used for the successful external lookup was not isolated.
- **Planned:** Windows 11 domain join, OUs, users, groups, and GPOs.

## Folders

- [`docs/`](docs/) — verified setup notes and future procedures.
- [`screenshots/`](screenshots/) — future redacted visual evidence.
- [`diagrams/`](diagrams/) — future network and system diagrams.
- [`powershell/`](powershell/) — future tested automation scripts.
