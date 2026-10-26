# Enterprise IT Support

Windows Server and client administration lab. The server setup is in progress; see [lab status](docs/README.md) for the configuration confirmed so far.

## Status

- **Completed:** Windows Server 2025 Standard Evaluation with Desktop Experience is running in VirtualBox as DC01. The NAT adapter has 10.0.2.15 via DHCP; internal Ethernet 2 is 192.168.100.10/24. The AD DS server role is installed.
- **Planned:** DNS configuration, promotion of DC01 to a domain controller, and a Windows 11 client.

The AD DS role installation alone does not mean DC01 is a domain controller. Promotion has not been completed.

## Folders

- [`docs/`](docs/) — verified setup notes and future procedures.
- [`screenshots/`](screenshots/) — future redacted visual evidence.
- [`diagrams/`](diagrams/) — future network and system diagrams.
- [`powershell/`](powershell/) — future tested automation scripts.
