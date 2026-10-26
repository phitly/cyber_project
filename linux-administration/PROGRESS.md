# Linux Systems Administration & Troubleshooting Lab — Progress

## 2026-10-26 — Phase 1 complete: server build and remote access

Phase 1 establishes a working Ubuntu Server VM, network connectivity, SSH access, and an updated package baseline. This log records the facts supplied and confirmed by the user; no commands were run against the VM while preparing this documentation. The date is the documentation checkpoint, not an independently verified installation date.

## Architecture

```text
Apple M1 MacBook Air (8 GB RAM, macOS SSH client)
  └─ UTM: LINUX01-Ubuntu-Server
       ├─ ARM64 Ubuntu Server
       ├─ 2 vCPU / 2 GB RAM / 25 GB virtual disk
       ├─ Guided entire virtual disk + LVM; no LUKS encryption
       └─ UTM shared/NAT networking → linux01, 192.168.64.3
            └─ SSH account: labadmin
```

- Installation media: ARM64 Ubuntu Server ISO; the reported screenshot filename was **`ubuntu-26.04.1-live-server-arm64.iso`**. This is media identification, not captured installed-release output. Do not relabel the installation as Ubuntu 24.04.
- Server name recorded during setup: **linux01**. Actual `hostnamectl` output has not been verified in this log.
- **labadmin** is the account indicated by the successful SSH connection command.
- **192.168.64.3** is the address used for the confirmed session; no static-address or DHCP-reservation configuration is established here.
- Shared/NAT networking supports the lab's host-to-guest SSH connection and guest outbound access. No public exposure or port-forwarding configuration is documented.

## Completed checkpoints

| Checkpoint | Recorded result and evidence boundary |
| --- | --- |
| VM provisioning | UTM VM created with the resources and ARM64 media listed above. |
| Installation | Guided use of the entire **virtual disk** with LVM; LUKS was not selected. |
| Installed-system boot | Initial boot loop resolved by detaching the installer ISO; server booted successfully afterward. |
| Network | IP-address and DNS-name pings succeeded, confirming basic IP reachability and name resolution. Exact ping targets and output are not retained here. |
| SSH service | sshd reported active; macOS SSH login to `labadmin@192.168.64.3` succeeded. Host fingerprint verification was reported complete; no fingerprint value is reproduced. |
| Dedicated key | An ED25519 key pair was generated at `~/.ssh/linux_lab_ed25519` on macOS; its public key was installed in Ubuntu's `authorized_keys`. |
| Key login | User confirmed successful key-based SSH login. A verbose SSH log showing `Authenticated ... using "publickey"` has not been independently verified or retained here. |
| Package maintenance | `apt update` and `apt upgrade` performed. User stated “no reboot required were up”; no reboot-required marker output was supplied. |

## Commands and purposes

These commands summarize the workflow and provide repeatable checks. Where exact command syntax or output was not retained, examples below are documentation aids, not a claim that every displayed command was executed verbatim.

### Ubuntu console or SSH session

| Command | Purpose |
| --- | --- |
| `ip addr` | Inspect interface addresses and identify the guest IP. |
| `ping -c 4 <known-reachable-IP>` | Test basic IP connectivity; replace the placeholder with the chosen test address. |
| `ping -c 4 <known-resolving-hostname>` | Test name resolution and connectivity to the selected host. |
| `systemctl status ssh` | Inspect Ubuntu's SSH service and confirm sshd is active. |
| `sudo apt update` | Refresh package indexes. |
| `sudo apt upgrade` | Install available package upgrades. |

### macOS SSH client

```sh
# Initial remote login; verify the host fingerprint before accepting it.
ssh labadmin@192.168.64.3

# Dedicated lab key generation (completed; do not overwrite an existing key).
ssh-keygen -t ed25519 -f ~/.ssh/linux_lab_ed25519

# Connect using the dedicated identity.
ssh -i ~/.ssh/linux_lab_ed25519 labadmin@192.168.64.3
```

Only the **public** key (`~/.ssh/linux_lab_ed25519.pub`) was installed in `/home/labadmin/.ssh/authorized_keys` on Ubuntu. The precise transfer command is not retained; no specific copy utility is claimed.

### Optional evidence follow-up — not yet verified

| Command | Purpose |
| --- | --- |
| `hostnamectl` and `cat /etc/os-release` | Capture actual hostname and installed OS release rather than infer them from setup or the ISO filename. |
| `ssh -v -o IdentitiesOnly=yes -i ~/.ssh/linux_lab_ed25519 labadmin@192.168.64.3` (macOS) | Capture an explicit public-key authentication result; review and redact verbose output before publishing. |
| `if [ -f /var/run/reboot-required ]; then cat /var/run/reboot-required; else echo "No reboot-required marker"; fi` (Ubuntu) | Record whether Ubuntu has a reboot-required marker; this check is not the evidence behind the user's earlier confirmation. |

## Troubleshooting record

| Issue | Action | Outcome |
| --- | --- | --- |
| Initial boot loop after installation | Detached the installation ISO in UTM and booted from the installed virtual disk. | Server booted successfully. No deeper root-cause diagnosis is claimed. |
| Validate network before remote administration | Checked IP and DNS pings, then the SSH service and macOS connection. | Basic networking and remote access confirmed. |
| Establish dedicated SSH credentials | Generated a lab-specific ED25519 key and installed its public key in the Ubuntu account. | Key login confirmed by the user; verbose authentication evidence remains optional follow-up. |

## Security precautions

- Never commit passwords, private keys, passphrases, tokens, or unredacted sensitive output. Key paths above identify locations only; no key material is included.
- Keep `~/.ssh/linux_lab_ed25519` private on macOS. Transfer only the `.pub` file to the server.
- Verify server host fingerprints through a trusted channel before accepting a new or changed host key.
- Restrict the guest's `.ssh` directory and `authorized_keys` to the account owner (typically modes 700 and 600). These permission checks are precautions, not a completed hardening claim.
- The virtual disk is **not LUKS-encrypted**. LVM is volume management and does not establish encryption.
- Password authentication disablement, root-login restrictions, firewall rules, and broader SSH hardening are not documented as completed.

## Next phase — users, groups, and permissions (planned)

- Create test users and groups; inspect identities and group membership with `id` and `groups`.
- Practice ownership and permissions with `chown`, `chgrp`, `chmod`, and `umask`.
- Build a group-owned shared directory and test allowed and denied access using separate accounts.
- Review least-privilege sudo access while preserving the working administrative login.
- Record commands, sanitized results, troubleshooting, and cleanup before marking Phase 2 complete.
