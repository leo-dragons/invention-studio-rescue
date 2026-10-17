# Invention Studio Rescue Utilities

<table>
  <tr>
    <td><img src="https://archive.org/download/tallerdeinventos/01front.jpg" width="350" height="450" style="object-fit: cover;"></td>
    <td><img src="https://archive.org/download/invention-studio-1996/6799050-invention-studio-windows-front-cover.jpg" width="350" height="450" style="object-fit: cover;"></td>
  </tr>
</table>

Run **Invention Studio** (Discovery Channel Multimedia, 1996, Spanish title
*Taller de Inventos*) on **Windows 10 and 11, 64-bit**: no CD, CD music
included, no crash in the garage, and optional full-screen HD scaling with your
own border artwork.

This release contains **only tools written for this project**. It contains no
part of the game. You need your own original CD (or a BIN/CUE image you made
from it).

**Authors:** leo_dragons and Claude (AI model by Anthropic)  
**Version:** 1.0 (October 2026)

---

## What it fixes

| Problem on modern Windows | Fix in this release |
|---|---|
| The installer only looks for the CD on drive `D:` | `INSTALL-NOCD.BAT` finds the disc on any drive and installs everything to the hard disk |
| No CD music | `winmm.dll` plays the CD audio tracks from a disc image or WAV file in the game folder |
| *CD Audio Error 26* at start-up | Same `winmm.dll`, once the music files are in place |
| The game crashes when you enter the garage | `patch-usinv3r.exe` (16-byte fix to your own copy of the game) |
| The game still needs the CD | `MAKE-PORTABLE.BAT` makes the game folder self-contained |
| Tiny 640 × 480 picture on modern monitors | `winmm.dll` scales the game to full screen (1080p, 1440p, 4K, ultrawide) with an optional border image |

Tested with the Spanish edition (*Taller de Inventos*, `USINV3R.EXE` version
1.0.0.1621) on Windows 11 Pro x64. See [Other editions](#other-editions).

---

## What you need

- Your original Invention Studio CD, or a BIN/CUE image made from it
- Windows 10 or 11 (64-bit)
- About 2 GB of free disk space while installing (the finished folder is about 550 MB)

No emulator and no compatibility settings are needed: the game itself is a
32-bit Windows program and runs directly.

---

## Files in this release

```
installer\
  INSTALL-NOCD.BAT        installs the game from the disc to C:\INVENT
game-folder\              everything here goes into C:\INVENT
  winmm.dll               CD music + HD scaling (loaded by the game automatically)
  cdaudio.ini             settings for winmm.dll
  patch-usinv3r.exe       fixes the garage crash in your USINV3R.EXE
  patch_usinv3r.py        the same fix as a readable Python script
  cue2wav.exe             turns the BIN/CUE image into a small WAV file
  MAKE-PORTABLE.BAT       makes the game folder self-contained (uses cue2wav.exe)
  pag2png.exe             exports the game's images for upscaling (optional)
  UPSCALE.BAT             runs Real-ESRGAN on the exported images (optional)
border-templates\         guides for designing your own border artwork
source\                   full source code and tests
```

---

## Installation (in this order)

### 1. Install the game from your disc

1. Put `INSTALL-NOCD.BAT` in the folder that holds your `.cue` and `.bin`
   files (or anywhere, if you use the physical CD).
2. Mount the image: double-click the `.cue` with a mounting tool, or insert the
   CD. Any drive letter works.
3. Double-click `INSTALL-NOCD.BAT`. Check that the drive or folder it reports
   is your disc, then press a key.

It then:
- copies the whole disc to `C:\INVENT\CD` and installs the game files to
  `C:\INVENT`, the same way the disc's own `INST32.BAT` does;
- writes `C:\INVENT\system\invent.ini` pointing at that copy;
- copies the `.cue`/`.bin` next to it into `C:\INVENT\music`, for the music;
- puts an **Invention Studio** shortcut on the desktop.

To name the disc yourself, run it from a Command Prompt:
`INSTALL-NOCD.BAT E:`

### 2. Copy the game-folder files

Copy everything from `game-folder\` into `C:\INVENT`, next to `USINV3R.EXE`.

### 3. Fix the garage crash

Double-click `patch-usinv3r.exe` in `C:\INVENT`.

It checks that your `USINV3R.EXE` is the expected version, keeps the original
as `USINV3R.EXE.orig`, and changes 16 bytes. If the file is a different
version, it changes nothing. To undo the fix, delete `USINV3R.EXE` and rename
`USINV3R.EXE.orig` back.

If you have Python 3, `patch_usinv3r.py` does exactly the same and you can
read every byte it changes first: open a Command Prompt in `C:\INVENT` and run
`python patch_usinv3r.py`. Both produce the same file (SHA-256 starting
`3e300e2c`).

### 4. Play

Start the game from the desktop shortcut. The music plays from the image in
`C:\INVENT\music`, so you can unmount the image. If `INSTALL-NOCD.BAT` was not
next to your `.cue`/`.bin`, copy them into `C:\INVENT\music` yourself.

### 5. Optional: make the folder self-contained

Double-click `MAKE-PORTABLE.BAT` in `C:\INVENT`. Nothing is deleted until it
has been verified. It:

- checks that every file of `C:\INVENT\CD` is also in `C:\INVENT`, then removes
  the duplicate `CD` folder;
- makes `invent.ini` use relative paths only;
- converts the ~600 MB BIN/CUE into `music\track02.wav` (~25 MB) plus a tiny
  `music.cue` that keeps the original disc layout. `cue2wav.exe` re-reads the
  result and compares every track position and every audio sample with the
  original before it deletes the `.bin` and `.cue`;
- refreshes the desktop shortcut.

Afterwards the folder holds everything the game needs. You can zip it and move
it to any hard disk, internal or external. Keep the path short and outside
`Program Files`, for example `C:\Games\INVENT`, because the game saves your
inventions in its own folder. Run `MAKE-PORTABLE.BAT` once more after moving it
to update the shortcut. The game checks that it runs from a hard disk, so USB
sticks may not work.

### 6. Optional: border artwork

Save an image as `C:\INVENT\border.png`. See [Border artwork](#border-artwork).

---

## HD display

`winmm.dll` scales the game's 640 × 480 picture to your monitor, keeping its
4:3 shape.

| Key | Action |
|---|---|
| **Ctrl+I** | Settings menu: display mode, scaling filter, border, mouse confinement |
| **Alt+Enter** | Switch between full screen and window |
| **Shift** while starting the game | Run without scaling, exactly as the original, for that session |

Scaling filters:

| Filter | Look |
|---|---|
| **Pixel** | Whole-number scale (4× on 4K, 2× on 1080p). Perfect square pixels; leaves a frame on all four sides |
| **Sharp** (default) | Fills the screen height. Crisp pixels with clean edges |
| **Smooth** | Fills the screen height. Soft look |

Settings are saved in the `[display]` section of `cdaudio.ini` and can also be
set there before starting the game. `enable=0` turns the scaling off for good.

The mouse, the in-game text box and the videos are scaled to match. Message
boxes and dialogs stay normal Windows size.

## Border artwork

Make it **7680 × 2160** and save it as `border.png` in the game folder. JPG and
BMP also work; set `border=yourfile.jpg` under `[display]` in `cdaudio.ini`.
The image is scaled so its height fills the screen and is centred; the sides
are cropped to fit, so one file works on every monitor shape.

| Canvas area (x) | Visible on |
|---|---|
| 2400 – 5280 | never: covered by the game (Sharp/Smooth filters) |
| 2112 – 2400 and 5280 – 5568 | every widescreen monitor, so keep essential art here |
| 1920 – 5760 | 16:9 screens (1080p, 1440p, 4K) |
| 1260 – 6420 | 21:9 ultrawide |
| 0 – 7680 | 32:9 super-ultrawide |

With the Pixel filter on 4K and 1080p screens, the game is smaller (x 2560–5120,
y 120–2040 on the canvas), so draw a frame edge around the game area.
`border-templates\border-template-UNIVERSAL-7680x2160.png` has all of these
lines marked.

After changing the image, use **Ctrl+I → Border → Reload the image**.
Transparent areas show as black. Please don't use the game's own artwork or
logos in a border you share; see [Sharing](#what-you-may-and-may-not-share).

---

## Upscaling the art (work in progress)

The export and upscaling steps work. Using the upscaled images inside the game
is **not finished yet**.

1. **Export:** run `pag2png.exe` in `C:\INVENT`. Every image from the game's
   `.PAG` files is written once to `hd-export\` as PNG, with
   `hd-manifest.csv` listing where each one appears on screen. Sprites drawn on
   a key colour (bright green, magenta, yellow…) are saved with that colour
   transparent, and the hidden pixels are filled with neighbouring colours so
   AI upscalers don't create coloured fringes.
2. **Upscale:** download `realesrgan-ncnn-vulkan-20220424-windows.zip` from the
   [Real-ESRGAN releases page](https://github.com/xinntao/Real-ESRGAN/releases)
   (v0.2.5.0) and unzip it into `C:\INVENT` next to `UPSCALE.BAT`. Then run:
   - `UPSCALE.BAT` for the anime model (flat, drawn art);
   - `UPSCALE.BAT realesrgan-x4plus` for the general model (painted or rendered art).

   The results go to `hd\` with the same file names. Try both models on a few
   images first: `UPSCALE.BAT realesrgan-x4plus test test-out`.

---

## Troubleshooting

`C:\INVENT\cdaudio.log` is rewritten every time the game runs. It shows where
the music was found, the track list, every CD command, and a `display:` line for
each scaling step. Include it when you ask for help, after removing anything
personal.

| Symptom | What to do |
|---|---|
| *CD Audio Error 26* or no music | The `.cue`/`.bin` (or `music.cue` + `track02.wav`) is not in `C:\INVENT\music`. The log shows where it looked |
| Crash entering the garage | Run `patch-usinv3r.exe` |
| `INSTALL-NOCD.BAT` cannot find the disc | Mount the image first, or run `INSTALL-NOCD.BAT E:` with the right letter |
| The game asks for the CD | Run `MAKE-PORTABLE.BAT`, or check `C:\INVENT\system\invent.ini` |
| Picture problems with the HD scaling | Hold **Shift** while starting the game, or set `enable=0` under `[display]` |
| Antivirus warning about `winmm.dll` | Replacement DLLs are sometimes flagged. Check the hashes in `SHA256SUMS.txt`, or build from `source\` |

## Other editions

`winmm.dll`, `cue2wav.exe` and `pag2png.exe` are generic and should work with
any edition. `INSTALL-NOCD.BAT` expects the disc layout of the edition we had
(`MPSYS.DLL` and `USINV3R.EXE` in the root). `patch-usinv3r.exe` only patches
`USINV3R.EXE` 1.0.0.1621 (238,080 bytes, SHA-256 starting `12406545`) and
refuses anything else. Reports from other editions are welcome, especially the
English one: size, version and SHA-256 of `USINV3R.EXE`.

---

## What you may and may not share

*This is general guidance, not legal advice.*

**This release** is original work by the authors and is published under the
MIT licence (see `LICENSE`). You may share and modify it.

**Do not share any part of the game:** the CD image (BIN/CUE/ISO), `.PAG`,
`.WAV` and `.DLL` files from the disc, `USINV3R.EXE` (original or patched),
`track02.wav` / `music.cue` made from the disc, or images exported with
`pag2png.exe` and their upscaled versions. They are still under copyright,
even though the game is no longer sold. "Abandonware" has no legal standing.
Everyone needs their own copy of the game; that is why this release ships a
patcher instead of a patched game.

**Your border artwork** is yours to share, as long as it doesn't include the
game's art, characters, or logos.

**Real-ESRGAN** is not included. Download it from its official page; it has
its own licence (BSD-3-Clause).

---

## How it works

- **No-CD install:** the disc's `INST32.BAT` assumes drive `D:` and leaves most
  data on the CD. `INSTALL-NOCD.BAT` copies the whole disc and points
  `invent.ini` at the copy.
- **CD music:** the game plays its music by sending MCI commands for the
  `cdaudio` device to `winmm.dll`. The replacement `winmm.dll` passes all other
  calls to the real Windows DLL, and answers the `cdaudio` commands itself:
  it builds a virtual disc from the BIN/CUE or WAV files and plays it through
  waveOut, with correct track positions and end-of-track notifications.
- **Garage crash:** before the garage opens, the game takes a snapshot of its
  screen and converts it to 256 colours. Its conversion code only handles 16-
  and 24-bit screens; on a 32-bit desktop it returns nothing, and the game
  crashes (access violation at `USINV3R.EXE+0x1B3A`). The patch routes every
  snapshot through a 24-bit capture and the game's own 24-bit converter.
- **HD display:** `winmm.dll` redirects the game's window and drawing calls.
  The game draws into a hidden 640 × 480 surface, which is scaled to the window
  with sharp-bilinear filtering. Mouse coordinates, the text box and the MCI
  video windows are mapped to the scaled picture.
- **Image format (`.PAG`):** a stream of word-aligned typed blocks. Block
  `0x20` holds a palette (BGR0 entries) and an image: run-length 8-bit, or raw
  1/4/8-bit. Details are in `source\src\pagfmt.h`.

## Building from source

Requirements: clang, lld, llvm-dlltool, Python 3 and the mingw-w64 headers and
`.def` files. No C runtime is linked.

```sh
cd source
MINGW_HEADERS=/path/to/mingw-w64-headers MINGW_DEFS=/path/to/mingw-w64-crt/lib32 sh build.sh
```

The tests (`tests\`) run on Linux with gcc. `tests/winshim` is a small POSIX
stand-in that runs the Windows tools' real source.

---

## Credits

- **leo_dragons:** testing on real hardware, investigation, artwork, direction
- **Claude** (AI model by Anthropic): reverse engineering, code, documentation

*Invention Studio* was developed by Music Pen and published by Discovery
Channel Multimedia. This project is not affiliated with or endorsed by them or
their successors. All trademarks belong to their owners.
