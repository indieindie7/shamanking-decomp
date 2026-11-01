# Shaman King: Spirit of Shamans (PlayStation, Bandai, 2002) reverse-engineering notes and symbols

Hand-named function map, naming prompt and tools for **Shaman King: Spirit of Shamans** (SLPS-03432, Japan):
the main executable `SLPS_034.32` (MIPS R3000 + GTE, PsyQ GCC, loaded at 0x80010000) and its five code
overlays `O_MINI`, `O_MOVIE`, `O_OPTION`, `O_STORY`, `O_TRAIN` (loaded at 0x800cad88, the end of the main bss).
Current state (after round 3):

| Program | Functions | Named | Provisional |
|---|---|---|---|
| SLPS_034.32 | 2,139 | 2,139 | 81 |
| O_MINI | 114 | 114 | 1 |
| O_STORY | 146 | 146 | 24 |
| O_TRAIN | 115 | 115 | 1 |
| O_OPTION | 40 | 40 | 0 |
| O_MOVIE | 28 | 28 | 3 |

0 `FUN_` placeholders left.

- Round 1: one naming pass over every function Ghidra found (2,339).
- Round 2 (11-01): 293 provisional names reviewed by 3 agents with callers/callees context: 231 confirmed, 56 renamed, 6 still `provisional`.
- Round 3 (11-01, coverage): `tools/orphans.py` scanned for code no function owned (pointer-table handlers, leaf functions after `jr ra`, jal targets in gaps). 243 missed functions created and named (SLPS +143, STORY +61, TRAIN +26, MINI +10, MOVIE +3); leftover refs checked to be data. New names still marked `provisional` where unsure.

Game: 3D arena fighter where shamans fight with a guardian spirit (over soul, furyoku spirit meter).
Subsystems named so far: fighters and character state machines (`fighter_`, `fgt_`, `char_`/`chr_`), spirits
(`spirit_`), hit detection (`hit_`), battle and versus flow (`battle_`, `vs_`), AI (`ai_`), bosses (`boss_`),
animation (`anim_`), effects (`fx_`), HUD (`hud_`), GTE / GPU drawing (`gte_`, `gpu_`, `gfx_`), sound and
sequences (`snd_`, `seq_`, `spu_`), CD / XA streaming (`cd_`), pad (`pad_`), memory card (`mc_`),
BIOS calls (`biosA0_` / `biosB0_` / `biosC0_`).

Same method as [rakugaki-decomp](https://github.com/indieindie7/rakugaki-decomp): Ghidra seeds functions from
MIPS prologues, `psxrules.py` names the BIOS stubs and trivial returns, a hinted and trimmed decompile is cut
into slices, each slice is named from its code and strings (prompt in `ghidra/naming/prompt_SK.txt`), names are
merged (`mergehand.py`) and applied as USER_DEFINED labels. Non-commercial, done for study and preservation.

**No game data is included**: no disc image, no executables, no overlays, no decompiled C, no game text,
no Ghidra database. You need your own copy of the game to use any of this.

## What is here

| Path | Content |
|---|---|
| `NOTES.md` | disc layout, the SOUL.FAT archive, overlay base, Ghidra setup, the naming round |
| `ghidra/hand_<prog>.txt` | hand names: `0x<addr> func <name> hand <comment>` (apply with ApplyNames.java from the Scorcher / AMOK repos, hand mode) |
| `ghidra/funcinfo_<prog>.tsv` | per-function export: `F` address name source size, `C` callees (string rows removed) |
| `ghidra/naming/prompt_SK.txt` | the round-1 naming prompt |
| `ghidra/review/` | round-2 review prompt + verdict lines (`SK 0x<addr> <old> CONFIRM/RENAME/UNSURE <new> <reason>`) |
| `ghidra/scripts/` | `SeedMipsFunctionsAt.java` (prologue seeding from the program's lowest address), `CsDumpAll.java` (decompile dump) |
| `tools/` | `psxrules.py`, `injecthints.py`, `trimdump.py`, `slice.py`, `mergehand.py`, `pubfuncinfo.py`, `orphans.py` (coverage scan) |

## Licence

Notes, names and tools: MIT (see `LICENSE`). Shaman King belongs to its rights holders; nothing from the
game is distributed here.
