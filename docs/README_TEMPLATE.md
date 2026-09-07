# Backup README Template

Every leaf folder in this repository (one machine, device, or piece of software) should
have a `README.md` following this structure. Copy the template below and fill in each
section; if a section doesn't apply, keep the heading and write `N/A`.

```markdown
# <Name>

**Description:** One-line summary of what this is and why it's backed up here.

## Source / Version

- Type: <machine | device | software>
- Firmware/App version: <version or "unknown">
- Last updated: <YYYY-MM-DD>

## Files

| File | Description |
| ---- | ----------- |
| `example.conf` | What it configures |

## Backup

Where the live config lives on the real machine/device/software, and how to export it.

## Restore

Steps to put the backed-up file(s) back in place (target path, command, required
packages, reboot/restart notes, etc).

## Notes

Anything else worth knowing: quirks, TODOs, gotchas.
```

## Guidelines

- Folder names use `snake_case`, lowercase.
- Hardware is split by `hardware/devices` (general-purpose devices: tablets, handhelds)
  vs `hardware/machines` (single-purpose machines: 3D printers, CNCs, laser engravers),
  further grouped by machine type where there's more than one of a kind
  (`machines/3d_printers`, `machines/cnc`, ...).
- Software backups live under `software/<app_name>`, unless the software is only ever
  used with one specific machine/device (e.g. LightBurn with the Sculpfun laser) — in
  that case it lives inside that machine/device's own folder instead.
- Each category folder (`hardware/`, `hardware/devices/`, `hardware/machines/`,
  `hardware/machines/3d_printers/`, `hardware/machines/cnc/`, `software/`) has its own
  short index `README.md` linking to its children — keep those in sync when adding or
  removing a folder.
- If a folder has no backup content yet, still add the README with what's known
  (source/version, expected files) and note in **Backup**/**Restore** that content is
  pending.
