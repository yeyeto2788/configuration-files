# LightBurn

**Description:** LightBurn preferences backup, used to control the
[Sculpfun S9 laser engraver](../README.md). Kept alongside the machine rather than under
`software/` since this config is specific to this one laser, not a general-purpose tool
install.

## Source / Version

- Type: software (machine-specific)
- Firmware/App version: LightBurn (preferences dated 2022-10-07)
- Last updated: 2026-09-07

## Files

| File | Description |
| ---- | ----------- |
| `20221007.lbprefs` | Full LightBurn preferences export (devices, cut settings, UI layout). |

## Backup

In LightBurn: `Edit > Preferences`, then export/copy the `.lbprefs` file from
LightBurn's settings folder (`%APPDATA%\LightBurn` on Windows,
`~/.config/LightBurn` on Linux).

## Restore

1. Install LightBurn.
2. Close LightBurn, replace its `.lbprefs` file with the one from this folder.
3. Restart LightBurn and verify the laser device and cut settings loaded correctly.

## Notes

Filename is the export date (`YYYYMMDD.lbprefs`); keep that convention for future
backups so history is easy to track.
