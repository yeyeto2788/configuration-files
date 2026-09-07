# FlatCAM

**Description:** FlatCAM configuration and shell-script workflows for turning PCB
gerber/excellon files into G-code for isolation milling and cutout.

## Source / Version

- Type: software
- Firmware/App version: FlatCAM (unknown version)
- Last updated: 2026-09-07

## Files

| File | Description |
| ---- | ----------- |
| `defaults.json` | FlatCAM defaults (untested import). |
| `Docs/current_configuration/defaults.json` | Snapshot of an in-use configuration. |
| `Docs/images/` | Screenshots referenced in this documentation. |

## Backup

`defaults.json` is FlatCAM's exported configuration/preferences file.

## Restore

Import `defaults.json` into FlatCAM via its preferences import option.

## Notes

**Possibly outdated:** the shell workflow below (`FlatCAM.py --shellfile`, TCL commands
like `open_gerber`/`isolate`/`cncjob`/`geocutout`) matches the classic Denvi/FlatCAM
project (Python 2, discontinued ~2019), not the actively maintained `flatcam.org`
FlatCAM 8.x fork. FlatCAM 8 still documents the same shell commands, but `defaults.json`
is a raw preferences dump tied to a specific build/fork and likely won't import cleanly
into a different one. TODO: confirm which FlatCAM you're actually running, record its
real version above, and re-export `defaults.json` from it.

### How to process some basic files

#### Processing the top/bottom layer

```
python FlatCAM.py --shellfile=/path/to/file
open_gerber mygerber.gbr -outname pcb
isolate pcb -tooldia 0.1 -passes 3 -overlap 0.05 -combine 1 -outname pcb_iso
cncjob pcb_iso -z_cut -0.1 -z_move 1.0 -feedrate 90.0 -tooldia 0.1 -outname pcb_iso_cnc
write_gcode pcb_iso_cnc pcb.gcode
export_gcode pcb.gcode
```

#### Processing the drill holes

```
python FlatCAM.py --shellfile=/path/to/file
open_excellon drill.drl -outname drl
drillcncjob drl -drillz -1.6 -travelz 1.5 -feedrate 60.0 -spindlespeed 1000 -outname drl_cnc
write_gcode drl_cnc drl.gcode
```

#### Processing the PCB cutout

```
python FlatCAM.py --shellfile=/path/to/file
open_gerber cutgerber.gbr -outname cuts
isolate cuts -dia 2.0 -passes 1 -overlap 0.0 -combine 1 -outname cuts_iso
ext cuts_iso -outname cuts_iso_exterior
delete cuts_iso
geocutout cuts_iso_exterior -dia 2.0 -gapsize 0.5 -gaps lr
cncjob cuts_iso_exterior -z_cut -1.6 -z_move 1.5 -feedrate 60.0 -tooldia 2.0 -spindlespeed 1000 -multidepth true -depthperpass 0.8 -outname pcb_cuts_cnc
write_gcode pcb_cuts_cnc cuts.gcode
```

Steps for the PCB cutout, in order:

1. **Import Geometry** — open `cutgerber.gbr`: `open_gerber cutgerber.gbr -outname cuts`
2. **Generate Isolation Geometry** — produces `cuts_iso`:
   `isolate cuts -dia 2.0 -passes 1 -overlap 0.0 -combine 1 -outname cuts_iso`
3. **Generate the exterior geometry** — produces `cuts_iso_exterior`:
   `ext cuts_iso -outname cuts_iso_exterior`
4. **Create the path and cutout for the board**:
   `geocutout cuts_iso_exterior -dia 2.0 -gapsize 0.5 -gaps lr`
   (`-gaps` accepts `8|4|tb|lr|2tb|2lr`)

### TODO

- Offset for the boards:
  ```
  offset pcb -12 198
  offset cuts -12 198
  offset drl -12 198
  ```
- Slow start G-code:
  ```
  G00 Z4
  G00 X0 Y0
  S0
  M03
  S200
  G4 P2
  S400
  G4 P2
  S600
  G4 P2
  ```
