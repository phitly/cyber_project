# Lab Status

See the [dated progress log](../PROGRESS.md) for checkpoints, exact verification commands, and next steps.

## Verified configuration through 2026-10-26

| Component | Current state |
| --- | --- |
| Virtualization | VirtualBox; 4 GB RAM, 2 CPUs, 60 GB dynamic VDI |
| Server OS | Windows Server 2025 Standard Evaluation, Desktop Experience |
| Server name | DC01 |
| NAT adapter | Ethernet, 10.0.2.15, gateway via NAT; DNS registration disabled |
| Internal adapter | Ethernet 2, Enterprise-Lab, 192.168.100.10/24, no gateway; DNS registration enabled |
| DNS client | Both interfaces point to 192.168.100.10 |
| AD DS | Role installed; new corp.example.test forest, CORP NetBIOS; promoted and restarted |
| Domain services | Domain sign-in confirmed; user reports NTDS, DNS, and Netlogon running |
| Guest Additions | Installed; clipboard working |

## Pending verification and work

Actual AD domain/controller command output, DNS A/SRV records, multihomed DNS registration and listening address, forwarding, Windows 11 domain join, and OUs/users/groups/GPOs. Record sanitized output before marking these complete.
