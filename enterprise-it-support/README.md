# Enterprise IT Support

Windows Server and client administration lab. See the [progress log](PROGRESS.md) for dated checkpoints, verification commands, and next steps.

## Status through 2026-10-26

- **Verified:** DC01 runs Windows Server 2025 Standard Evaluation (Desktop Experience) in VirtualBox. NAT Ethernet has 10.0.2.15; internal Ethernet 2 has 192.168.100.10/24. The AD DS role is installed; DC01 was promoted into the corp.example.test forest, restarted, and signed in to the domain. The user confirmed NTDS, DNS, and Netlogon are running.
- **Pending verification:** actual `Get-ADDomain` and `Get-ADDomainController` output, AD DNS A/SRV records, multihomed DNS registration and listening address, and DNS forwarding.
- **Planned:** Windows 11 domain join, OUs, users, groups, and GPOs.

## Folders

- [`docs/`](docs/) — verified setup notes and future procedures.
- [`screenshots/`](screenshots/) — future redacted visual evidence.
- [`diagrams/`](diagrams/) — future network and system diagrams.
- [`powershell/`](powershell/) — future tested automation scripts.
