# Lab Status

See the [dated progress log](../PROGRESS.md) for checkpoints, exact verification commands, and next steps.

## Verified configuration through 2026-10-27

| Component | Current state |
| --- | --- |
| Virtualization | VirtualBox; 4 GB RAM, 2 CPUs, 60 GB dynamic VDI |
| Server OS | Windows Server 2025 Standard Evaluation, Desktop Experience |
| Server name | DC01 |
| NAT adapter | Ethernet, 10.0.2.15, gateway via NAT; DNS registration disabled |
| Internal adapter | Ethernet 2, Enterprise-Lab, 192.168.100.10/24, no gateway; DNS registration enabled |
| DNS client | Both interfaces point to 192.168.100.10 |
| AD DS | Role installed; new corp.example.test forest, CORP NetBIOS; promoted and restarted |
| Domain services | Domain sign-in confirmed; NTDS, DNS, and Netlogon observed running |
| DNS checks | AD locator SRV present; DC01 host A record and domain/GC A records return 192.168.100.10 after unwanted NAT A record removal; external lookup succeeds |
| Guest Additions | Installed; clipboard working |
| Post-restart checks | DC01 passed Advertising; authoritative DC01 A lookup returned only 192.168.100.10; NETLOGON and SYSVOL shares were present; Windows Update handled per user report |

## Pending verification and work

`dcdiag /test:DNS /DnsRecordRegistration` passed its main DNS test with warnings for the intentionally unregistered DHCP NAT adapter. Remaining AD properties, the specific external DNS resolution path, Windows 11 domain join, and OUs/users/groups/GPOs are not yet documented as complete.
