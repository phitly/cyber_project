# Enterprise IT Support & Hybrid Identity Lab — Progress Log

Last updated: 2026-10-26

This log records the lab state reported and confirmed through 2026-10-26. **Verified** means the user confirmed the configuration or result during setup; it does not imply that command output or screenshots have been committed. **Pending** means a result has not yet been supplied or a task has not yet been completed. No passwords, recovery keys, tokens, or other secrets are recorded here.

## Dated checkpoints

### 2026-10-26 — Phase 1: server VM (verified)

- Created **DC01** in VirtualBox with **Windows Server 2025 Standard Evaluation (Desktop Experience)**.
- Allocated **4 GB RAM**, **2 CPUs**, and a **60 GB dynamically allocated VDI**.
- Installed VirtualBox Guest Additions; shared clipboard is working.

### 2026-10-26 — Phase 2: network and AD DS preparation (verified)

- **Ethernet** uses VirtualBox NAT and received **10.0.2.15** with a default gateway through NAT.
- **Ethernet 2** uses the VirtualBox **Internal Network** named **Enterprise-Lab** and has **192.168.100.10/24** with no default gateway.
- Both interfaces use **192.168.100.10** as their DNS client address. DNS registration is disabled on the NAT interface and enabled on the internal interface.
- Installed the **Active Directory Domain Services (AD DS)** server role.

### 2026-10-26 — Phase 3: first domain controller (verified)

- Promoted DC01 into a new forest, **corp.example.test**, with NetBIOS name **CORP**; DNS server and Global Catalog were selected. DC01 restarted and the user signed in to the domain.
- Observed `Get-ADDomain | Select-Object DNSRoot,NetBIOSName`: `corp.example.test`, `CORP`.
- Observed `Get-ADDomainController | Select-Object HostName,IPv4Address,IsGlobalCatalog`: `DC01.corp.example.test`, `192.168.100.10`, `True`.
- Observed `Get-Service NTDS,DNS,Netlogon | Select-Object Name,Status`: all three **Running**.
- `Resolve-DnsName _ldap._tcp.dc._msdcs.corp.example.test -Server 192.168.100.10 -Type SRV` returned `dc01.corp.example.test` on port **389**. The `corp.example.test` and `gc._msdcs.corp.example.test` A lookups both returned **192.168.100.10**.
- Found two `DC01` host A records: **192.168.100.10** and unwanted NAT address **10.0.2.15**. `Get-DnsClient` confirmed registration disabled on Ethernet (NAT) and enabled on Ethernet 2. The user confirmed DNS Manager's server **Interfaces** and the zone **Name Servers** list use **192.168.100.10** only; the unwanted NAT A record was removed. A later `Get-DnsServerResourceRecord -ZoneName corp.example.test -Name DC01 -RRType A` listed only **192.168.100.10**, and `nslookup dc01.corp.example.test 192.168.100.10` returned only **192.168.100.10**. An earlier `Resolve-DnsName` result still showed both addresses; whether that was cached was not established.
- `Get-DnsServerForwarder` reported **fd17:625c:f037:2::3** and `UseRootHint: True`. `Resolve-DnsName example.com -Server 192.168.100.10` returned public A and AAAA answers. External name resolution works; these outputs alone do not establish whether the forwarder or root hints supplied the answer.

## Remaining checks before the Windows 11 domain join

1. Recheck the DC01 host record after a restart or later registration cycle, using both authoritative record inspection and a direct DNS query:
   ```powershell
   Get-DnsServerResourceRecord -ZoneName corp.example.test -Name DC01 -RRType A
   nslookup dc01.corp.example.test 192.168.100.10
   ```
   Both should show **192.168.100.10** only.
2. If a complete AD inventory is needed, capture the properties not included in the selections above:
   ```powershell
   Get-ADDomain | Select-Object Forest, DomainMode, PDCEmulator
   Get-ADDomainController -Identity DC01 | Select-Object Domain, Forest
   ```
3. Confirm the Windows 11 client can reach **192.168.100.10** and resolve the DC locator SRV record through that DNS server. The client join and OUs, users, groups, and GPOs remain pending.

## Next phases

- **Windows 11 client — pending:** attach it to **Enterprise-Lab**, set its DNS server to **192.168.100.10**, verify DC discovery, then join **corp.example.test** and record the result. On the client, after connectivity and DNS checks:
  ```powershell
  Resolve-DnsName _ldap._tcp.dc._msdcs.corp.example.test -Server 192.168.100.10 -Type SRV
  Add-Computer -DomainName corp.example.test -Credential (Get-Credential 'CORP\Administrator') -Restart
  ```
  Enter credentials only in the secure prompt; never commit them.
- **Directory structure and policy — pending:** design and create OUs, users, groups, and GPOs; test and document each result.
- Update this log after each phase with the date, commands run, observed results, and redacted evidence. Do not publish passwords or secrets.
