# Shaman King: Spirit of Shamans (PlayStation, Bandai, 2002) reverse-engineering notes and symbols

Hand-named function map, naming prompt and tools for **Shaman King: Spirit of Shamans** (SLPS-03432, Japan):
the main executable `SLPS_034.32` (MIPS R3000 + GTE, PsyQ GCC, loaded at 0x80010000) and its five code
overlays `O_MINI`, `O_MOVIE`, `O_OPTION`, `O_STORY`, `O_TRAIN` (loaded at 0x800cad88, the end of the main bss).
Round 1 (one naming pass over every function):

| Program | Functions | Named | Provisional |
|---|---|---|---|
| SLPS_034.32 | 1,996 | 1,996 | 269 |
| O_MINI | 104 | 104 | 1 |
| O_STORY | 85 | 85 | 0 |
| O_TRAIN | 89 | 89 | 0 |
| O_OPTION | 40 | 40 | 0 |
| O_MOVIE | 25 | 25 | 0 |

0 `FUN_` placeholders left. Names marked `provisional` in the comment are best guesses for a later review round.

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
| `ghidra/scripts/` | `SeedMipsFunctionsAt.java` (prologue seeding from the program's lowest address), `CsDumpAll.java` (decompile dump) |
| `tools/` | `psxrules.py`, `injecthints.py`, `trimdump.py`, `slice.py`, `mergehand.py`, `pubfuncinfo.py` |

## Licence

Notes, names and tools: MIT (see `LICENSE`). Shaman King belongs to its rights holders; nothing from the
game is distributed here.
