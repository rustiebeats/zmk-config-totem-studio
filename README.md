# TOTEM — ZMK Firmware Config

[![Build ZMK firmware](../../actions/workflows/build.yml/badge.svg)](../../actions/workflows/build.yml)

> Miryoku-inspired ZMK configuration for the [TOTEM](https://github.com/GEIGEIGEIST/TOTEM) 38-key split keyboard. QWERTY base, vim-style navigation, home row mods, mouse keys, combos, and [ZMK Studio](https://zmk.studio/) support for real-time keymap editing.

---

## Keymap

![Keymap](keymap-drawer/totem.svg)

* Auto-generated with [keymap-drawer](https://github.com/caksoylar/keymap-drawer) on every push.*

---

## Layout Philosophy

This config follows the [Miryoku](https://github.com/manna-harbour/miryoku) layout system — a minimal 36-key layout that puts everything within one key of the home row.

- **QWERTY** base with **home row mods** (GACS order)
- **Vim-style** navigation (HJKL arrows)
- **6 thumb-activated layers** — hold a thumb key to access NAV, NUM, SYM, MEDIA, FUN, or MOUSE
- **Symmetric design** — left thumbs activate right-hand layers and vice versa
- **Mouse keys** — full mouse emulation (movement, scroll, click) on the MOUSE layer
- **14 combos** — Caps Word, Esc, Bspc, Tab, Enter, clipboard, and vertical symbol combos
- **Extra keys** — Ctrl/Esc (left) and Hyper (right) on the outer pinky positions

### Thumb Keys

```
Left hand                           Right hand
┌───────────┬───────────┬────────┐ ┌─────────┬──────────┬─────────┐
│ MEDIA/ESC │  NAV/SPC  │MOUSE/TAB│ │ SYM/ENT │ NUM/BSPC │ FUN/DEL │
└───────────┴───────────┴────────┘ └─────────┴──────────┴─────────┘
```

### Extra Keys

| Position | Tap | Hold | Use Case |
|:---------|:----|:-----|:---------|
| Left outer | Esc | Ctrl | Vim escape + Ctrl shortcuts without leaving home row |
| Right outer | — | Hyper (GUI+Alt+Ctrl+Shift) | Launcher shortcuts (Raycast, Alfred, etc.) with a clean modifier namespace |

### Layers

| Layer | Activation | Left Hand | Right Hand |
|:------|:-----------|:----------|:-----------|
| BASE | Default | QWERTY + home row mods | QWERTY + home row mods |
| NAV | Hold `Space` | Mods | Vim arrows (HJKL), Cmd clipboard, PgUp/PgDn, Home/End |
| MOUSE | Hold `Tab` | Mods | Mouse movement (HJKL), scroll, L/M/R click |
| MEDIA | Hold `Esc` | Mods | Prev/Next, Vol Up/Down, Play/Stop, BT profiles |
| NUM | Hold `Bspc` | `[ 7 8 9 ]`, `; 4 5 6 =`, `` ` 1 2 3 \ `` | Mods |
| SYM | Hold `Enter` | `{ & * ( }`, `: $ % ^ +`, `~ ! @ # \|` | Mods |
| FUN | Hold `Del` | F12-F1, PrtSc, ScrLk, Pause/Break | Mods |

### Combos

| Combo | Keys | Output | Type |
|:------|:-----|:-------|:-----|
| W + E | top left | Escape | Horizontal |
| I + O | top right | Backspace | Horizontal |
| O + P | top right | Delete | Horizontal |
| F + J | index home | Caps Word | Horizontal |
| S + D | home left | Tab | Horizontal |
| K + L | home right | Enter | Horizontal |
| X + C | bottom left | Cmd+C (Copy) | Horizontal |
| C + V | bottom left | Cmd+V (Paste) | Horizontal |
| X + V | bottom left | Cmd+X (Cut) | Horizontal |
| J + M | right index | `-` (minus) | Vertical |
| H + N | right inner | `_` (underscore) | Vertical |
| F + V | left index | `=` (equal) | Vertical |
| S + X | left ring | `` ` `` (grave) | Vertical |
| L + ' | right outer | `;` (semicolon) | Horizontal |

All combos use `require-prior-idle-ms` to prevent misfires during fast typing.
 
### Home Row Mods

```
Left:   GUI / A    ALT / S    CTRL / D    SHFT / F
Right:  SHFT / J   CTRL / K   ALT / L     GUI / '
```

| Setting | Value |
|:--------|:------|
| Flavor | tap-preferred |
| Tapping term | 200 ms |
| Quick tap | 175 ms |
| Require prior idle | 150 ms |

---

## Hardware

| | |
|:--|:--|
| **Keyboard** | [TOTEM](https://github.com/GEIGEIGEIST/TOTEM) — 38-key columnar stagger split |
| **MCU** | Seeeduino XIAO BLE (nRF52840) |
| **Firmware** | [ZMK](https://zmk.dev/) main branch (Zephyr 4.1) |
| **Features** | Mouse keys, ZMK Studio, Bluetooth |

---

## Getting the Firmware

Firmware builds automatically via GitHub Actions on every push.

1. Go to the [Actions](../../actions) tab
2. Open the latest successful **Build ZMK firmware** run
3. Download the `totem_left` and `totem_right` artifacts (`.uf2` files)

### Flashing

1. Double-tap the reset button on the XIAO BLE to enter bootloader mode
2. A USB drive will appear — drag the `.uf2` file onto it
3. Flash left half first (`totem_left`), then right (`totem_right`)

---

## ZMK Studio

[ZMK Studio](https://zmk.studio/) lets you edit your keymap in real time through a browser — no reflashing, no rebuilding, no code.

### What You Can Do

- Remap any key on any layer
- Add, remove, or reorder layers
- Configure hold-tap behaviors
- Test changes instantly — they apply the moment you make them
- Save layouts that persist across reboots

### How to Connect

1. **Plug in the left half** via USB (Studio is only enabled on the left half)
2. Open [zmk.studio](https://zmk.studio/) in a WebUSB-compatible browser (Chrome, Edge, or Brave)
3. Click **Connect** and select your TOTEM from the device list
4. Your current keymap loads automatically — start editing

### How It Works

- Changes apply **instantly** to the keyboard — no compile or flash step
- Edits are saved to the keyboard's **onboard flash**, so they survive reboots and unplugging
- The `.keymap` file in this repo is the **default layout** — Studio overrides sit on top of it
- To reset back to the defaults from this repo, use the **Restore Stock Settings** option in Studio

### Connecting via Bluetooth

You can also connect to Studio over BLE:

1. Make sure the left half is **not** plugged in via USB
2. Open [zmk.studio](https://zmk.studio/) and click **Connect via Bluetooth**
3. Pair with the TOTEM if prompted
4. Edit your keymap wirelessly

> **Note:** WebBluetooth support varies by OS. Chrome on macOS/Linux works best. Windows may require flags.

### Build Config

Studio support is configured in `build.yaml` for the left half:

```yaml
- board: xiao_ble//zmk
  shield: totem_left
  snippet: studio-rpc-usb-uart
  cmake-args: -DCONFIG_ZMK_STUDIO=y -DCONFIG_ZMK_STUDIO_LOCKING=n
```

- `CONFIG_ZMK_STUDIO=y` — enables the Studio RPC endpoint
- `CONFIG_ZMK_STUDIO_LOCKING=n` — disables the security lock so you don't need to confirm on the keyboard each time
- `snippet: studio-rpc-usb-uart` — routes Studio communication over USB serial

---

## Firmware Config

Key settings in `config/totem.conf`:

| Setting | Value | Purpose |
|:--------|:------|:--------|
| `CONFIG_ZMK_MOUSE` | `y` | Enable mouse key emulation (movement, scroll, click) |
| `CONFIG_ZMK_USB_LOGGING` | `n` | Disable USB logging for production use |

---

## Repository Structure

```
├── config/
│   ├── totem.keymap        # Keymap definition (layers, combos, macros)
│   ├── totem.conf          # Firmware config (mouse keys, logging)
│   └── west.yml            # ZMK module manifest (main branch)
├── boards/shields/totem/   # Board shield definition (matrix, pins, layout)
├── keymap-drawer/          # Auto-generated keymap diagrams
│   ├── config.yaml         # Diagram styling + mouse key labels
│   ├── totem.yaml          # Parsed keymap data
│   └── totem.svg           # Visual keymap diagram
├── build.yaml              # GitHub Actions build matrix
└── .github/workflows/
    ├── build.yml            # Firmware build workflow
    └── draw-keymaps.yml     # Keymap diagram generator
```

---

## Credits

- [TOTEM](https://github.com/GEIGEIGEIST/TOTEM) keyboard by GEIGEIGEIST
- [Miryoku](https://github.com/manna-harbour/miryoku) layout by Manna Harbour
- [ZMK Firmware](https://zmk.dev/)
- [keymap-drawer](https://github.com/caksoylar/keymap-drawer) by caksoylar
- [urob/zmk-config](https://github.com/urob/zmk-config) — combo and home row mod tuning reference
