# Anycubic i3 Mega

**Description:** Configuration, firmware, and reference files for the Anycubic i3 Mega
3D printer.

## Source / Version

- Type: machine (3D printer)
- Firmware/App version: Mega.hex ULTRABASE V1.1.2 / Cura 15.04.6
- Last updated: 2026-09-07

## Files

| Folder | Description |
| ------ | ----------- |
| `cura_profile/Default_PLA.ini` | Cura print profile for PLA. |
| `GCODE/Owl_pair.gcode` | Sample sliced G-code. |
| `STL/owl_pair.stl` | Sample model. |
| `original_firmware/` | Stock ULTRABASE V1.1.2 firmware `.hex` and the Cura upload instructions PDF. |
| `original_files_english/` | Vendor-supplied Cura installer, CP2102 USB drivers (mac/win), and manuals. |

## Backup

`cura_profile/Default_PLA.ini` is exported from Cura's print profile manager.

## Restore

1. Install Cura 15.04.6 (`original_files_english/Cura/Windows/Cura_15.04.6.exe`) or a
   newer version.
2. Install the CP2102 USB-to-serial driver for your OS from
   `original_files_english/Driver_CP2102/`.
3. Import `cura_profile/Default_PLA.ini` as a print profile in Cura.
4. To reflash stock firmware, upload `original_firmware/Mega.hex_ULTRABASE_V1.1.2.hex`
   following `original_firmware/The steps of uploading the hex firmware by cura.pdf`.

## Notes

`GCODE/Owl_pair.gcode` and `STL/owl_pair.stl` are a known-good sample print, useful for
verifying the printer works after a config/firmware restore.
