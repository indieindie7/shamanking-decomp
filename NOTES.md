# Notes

## Disc
- Data track MODE2/2352; files: `SLPS_034.32` (main exe, 733,184 bytes), `SOUL.BIN` + `SOUL.FAT` (packed data), `SOUL.XAB` + `SOUL.XAF` (XA audio), `SKING.STR` (movie), `SYSTEM.CNF`.
- PS-X EXE header: pc 0x800696d0, t_addr 0x80010000, t_size 0xb2800. Import the file from offset 0x800 at 0x80010000.
- The entry code clears bss 0x800c23a8..0x800cad88; overlays and heap start at 0x800cad88.

## SOUL.FAT
- 16-byte entries: `name[12]` + u32 LE v; sector = v >> 12, size = (v & 0xfff) sectors of 2048 bytes into SOUL.BIN.
- 639 entries: 524 XAP, 48 TIM, 36 AO (per-character data), 12 SEQ, 11 TIP, 5 PRG (code overlays), 3 EFP.
- Overlay load base 0x800cad88 was confirmed by scoring jal targets against function prologues.

## Ghidra
- Ghidra 12.1.4, BinaryLoader, `MIPS:LE:32:default`, base 0x80010000 (main) / 0x800cad88 (overlays), pre-script `SeedMipsFunctionsAt.java`.
- Seeds: main 1945, O_MINI 104, O_TRAIN 88, O_STORY 71, O_OPTION 40, O_MOVIE 25; after analysis 1996 / 104 / 89 / 85 / 40 / 25 functions.
- `psxrules.py` reads a 2 MB RAM image (main exe at 0x10000, overlay at 0xcad88) and the funcinfo export.

## Naming round 1 (2026-11-01)
- Main decompile cut into 8 slices (209-278 functions each), overlays one slice each; one naming pass per slice.
- `mergehand.py`: rule names win, first name per address kept, duplicate names get an `_<addr>` suffix.
- Result: every function named, 270 provisional.

## Next
- Review the provisional names with their callers.
- Locate fighter stat tables (life, furyoku gain/costs, damage, hitstun) in the main exe data.
