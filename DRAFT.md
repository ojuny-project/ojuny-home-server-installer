# DRAFT - ojuny-home-server-installer

Status: draft scope. Nothing is built. This file describes what the repository
is supposed to contain, not what it contains today.

## Purpose

The appliance installer for `ojuny-home-server-template`. The target journey is:
burn the image to a USB, boot the machine, answer a small number of questions,
and arrive at the web panel login without touching a shell.

## What the repository will contain

```text
ojuny-home-server-installer/
  flake.nix                  ISO and image outputs
  nixos/                     live profiles for each variant
  installer/                 appliance install flow
    disks/                   target and data disk selection
    network/                 hostname, address, DNS
    admin/                   first administrator account
    services/                default service selection
    handoff/                 panel URL and token
  docs/                      install guide, storage, recovery
  tests/                     VM and hardware checklists
```

### Install media

- Attended online ISO.
- Attended offline ISO with the Home Server closure.
- Attended discovery ISO using local Ojuny cache peers.
- Unattended ISO with a declarative answer file.
- A raw disk image for dedicated small-form-factor hardware.

### Install flow

1. Boot the media and select the Home Server template.
2. Select the system disk. Never select a data disk by accident.
3. Optionally encrypt the system disk.
4. Configure hostname and network.
5. Create the first administrator account.
6. Optionally select data disks for the storage pool.
7. Install and reboot.
8. Print or display the panel URL and a one-time setup token.
9. Complete the first-run wizard in the browser.
10. Enable services from the panel. Nothing beyond the panel is on by default.

### First-run wizard

- Confirm the administrator account and password.
- Create the storage pool and the first share if a data disk was selected.
- Show every optional service as disabled.
- Enable services one at a time, with DNS and the proxy offered as options.
- Configure the VPN and show the port-forward requirement if one is needed.
- Show the service dashboard with every enabled service linked.
- Explain how to add an Ojuny client share patch to a desktop or server.

### Recovery

- A panel recovery mode that starts the panel and SSH only.
- A factory reset that reinstalls the system but leaves data disks untouched by
  default.
- A disk replacement flow that does not wipe the surviving data disks.
- A documented restore from the configured backup target.

### Documentation

- The install journey and the minimum hardware.
- Disk selection, pools, encryption and data-disk safety.
- Network modes, and why DNS is optional until the user enables it.
- The service matrix and what each panel toggle enables.
- Panel URL, token handling and password recovery.
- Offline, discovery and unattended variants.
- Recovery and factory reset.

### Tests

- Each ISO boots in QEMU.
- An install reaches the panel login without a shell.
- A default install exposes only the panel and no optional service.
- The first-run wizard creates a pool and a share.
- Enabling DNS and the proxy makes them resolve the panel and every enabled
  service.
- A service toggle survives a reboot.
- A data disk is not wiped by a factory reset.
- An offline install completes without network access.
- The unattended path produces the same panel state as the attended path.

## Limits and non-goals

- `x86_64-linux` only.
- The unattended path wipes the selected system disk.
- No Wi-Fi configuration in the unattended path.
- No automatic port forwarding.
- No high-availability or cluster install.
- No data recovery from a failed disk.
- No promise that every service can be configured from the panel.
- No cloud account and no remote install service.
- No service enabled by default beyond what the panel needs.

## Open questions

- Is the install flow terminal-based or a local graphical wizard?
- Is the panel reachable by IP during first run, or only by generated DNS name?
- How is the one-time setup token delivered and invalidated?
- Are data disks formatted by the installer or by the first-run wizard?
- Does the installer support importing an existing ZFS or Btrfs pool?
- Is there a separate recovery image, or only a boot entry?
- Should the installer offer any service toggles, or leave every service to the
  panel?

## Sources

- `ojuny-installer/README.md` and `docs/offline-install.md`,
  `docs/unattended-install.md`, `docs/persistent-image.md`.
- `ojuny-template/docs/03-host-setup.md`.
- `nix-cache-discovery/README.md`.
