# WinUtils

**Description:** Config presets for [Chris Titus Tech's WinUtil](https://github.com/ChrisTitusTech/winutil),
used to script Windows app installs/tweaks after a fresh setup.

## Source / Version

- Type: software
- Firmware/App version: WinUtil, current flat-array config schema (as of 2026-09)
- Last updated: 2026-09-14

## Files

| File | Description |
| ---- | ----------- |
| `basic.json` | Minimal set: AnyDesk, Flameshot, FoxPDF Reader, LibreOffice, PDF24 Creator, VLC, plus NFS/Hyper-V/WSL features and a handful of privacy/cleanup tweaks. |
| `basic_personal.json` | Fuller personal set, superset of `basic.json`'s apps: adds Angry IP Scanner, Bitwarden, Chromium, Deskflow, Edge, Firefox, Git, Go, LocalSend, NAPS2, Node.js LTS, PowerShell, PuTTY, Rufus, Telegram, Windows Terminal, uv, VS Code, VSCodium, plus its own tweak selection. |

## Backup

Both files are flat JSON arrays of WinUtil checkbox IDs (`WPFInstall*`, `WPFFeature*`,
`WPFTweaks*`) — the current WinUtil export format. Export via WinUtil's config
export option, or edit the JSON directly (keep `basic.json`'s app list a subset of
`basic_personal.json`'s when both should include the same app).

## Restore

1. Run WinUtil (`irm christitus.com/win | iex` in an elevated PowerShell).
2. Use its config import option and point it at `basic.json` or `basic_personal.json`.
3. Run the install.

## Notes

`basic.json` is the baseline for any machine; `basic_personal.json` is the superset used
on personal machines. Both were re-exported/converted to WinUtil's current flat-array
schema on 2026-09-14, so the earlier legacy-schema staleness note no longer applies —
re-check periodically against WinUtil's current app/tweak IDs since they can still be
renamed or retired between releases.
