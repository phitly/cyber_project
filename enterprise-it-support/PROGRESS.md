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

### 2026-10-26 — Phase 3: first domain controller (verified progress)

- Promoted DC01 into a new forest, **corp.example.test**, with NetBIOS name **CORP**; DNS server and Global Catalog were selected.
- DC01 restarted and the user signed in to the domain.
- The user confirmed that the **NTDS**, **DNS**, and **Netlogon** services are running. Exact `Get-Service` output was not retained in this log.

## Phase 3 verification still pending

The promotion and sign-in are confirmed, but the following outputs and checks have not been supplied. Do not mark them complete until observed on DC01 and documented with sanitized evidence.

1. Capture the actual domain and domain-controller properties:
   ```powershell
   Get-ADDomain | Select-Object DNSRoot, NetBIOSName, Forest, DomainMode, PDCEmulator
   Get-ADDomainController -Identity DC01 | Select-Object HostName, Domain, Forest, IPv4Address, IsGlobalCatalog
   Get-Service NTDS, DNS, Netlogon | Select-Object Name, Status
   ```
2. Verify that DC01 resolves on the internal address and that the AD locator SRV record exists:
   ```powershell
   Resolve-DnsName dc01.corp.example.test -Server 192.168.100.10 -Type A
   Resolve-DnsName _ldap._tcp.dc._msdcs.corp.example.test -Server 192.168.100.10 -Type SRV
   ```
   The DC01 A record should point to **192.168.100.10**. Investigate any **10.0.2.15** A record for DC01.
3. Check the multihomed DNS configuration: confirm registration is disabled on Ethernet and enabled on Ethernet 2, and review which local addresses the DNS server listens on:
   ```powershell
   Get-DnsClient | Select-Object InterfaceAlias, RegisterThisConnectionsAddress
   Get-DnsClientServerAddress -AddressFamily IPv4 | Select-Object InterfaceAlias, ServerAddresses
   Get-DnsServerSetting -All | Select-Object ListenAddresses
   ```
   Confirm internal clients can query **192.168.100.10**; review the DNS server's **Interfaces** tab if the listening-address output needs clarification.
4. Inspect DNS forwarding and test external resolution through DC01 before recording it as configured:
   ```powershell
   Get-DnsServerForwarder
   Resolve-DnsName example.com -Server 192.168.100.10
   ```

## Next phases

- **Windows 11 client — pending:** attach it to **Enterprise-Lab**, set its DNS server to **192.168.100.10**, verify DC discovery, then join **corp.example.test** and record the result. On the client, after connectivity and DNS checks:
  ```powershell
  Resolve-DnsName _ldap._tcp.dc._msdcs.corp.example.test -Server 192.168.100.10 -Type SRV
  Add-Computer -DomainName corp.example.test -Credential (Get-Credential 'CORP\Administrator') -Restart
  ```
  Enter credentials only in the secure prompt; never commit them.
- **Directory structure and policy — pending:** design and create OUs, users, groups, and GPOs; test and document each result.
- Update this log after each phase with the date, commands run, observed results, and redacted evidence. Do not publish passwords or secrets.
