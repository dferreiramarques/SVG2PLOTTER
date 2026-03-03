# ✂ SVG2Plotter

**Visual Layout Manager for HPGL Vinyl Cutters**  
_Seikitech SK1350 · HPGL Protocol · Windows / macOS / Linux_

> Developed by **David Marques** — *Vibe Coding with Claude.ai*  
> Centro de Inovação Carlos Fiolhais · CDI Portugal

---

## What is this?

SVG2Plotter is a desktop application to control HPGL-compatible vinyl cutters directly from SVG files — no proprietary software required. It was built specifically for the **Seikitech SK1350** but works with any plotter that speaks HPGL over a serial connection.

The app replaces the vendor CD-ROM driver workflow with a clean Python GUI that handles SVG parsing, multi-file layout management, scale control, and direct serial communication.

![SVG2Plotter Interface](docs/screenshot.png)

---

## Features

- **Multi-SVG layout** — load multiple files, arrange side by side, drag to reposition
- **Full SVG transform support** — correctly handles Inkscape `matrix()`, `translate()`, `rotate()`, groups and layers
- **Scale tool** — save a scale value, then apply it to any SVG independently
- **Normal / Mirror mode** — cut for front-facing surfaces or glass/window application
- **Live preview** — canvas with grid, rulers, zoom/pan, and path rendering
- **Pure Python** — no proprietary dependencies, runs on Windows, macOS, Linux
- **Installer included** — `setup.py` installs deps, generates icon, creates Desktop shortcut

---

## Requirements

| | |
|---|---|
| Python | 3.8 or newer ([python.org](https://python.org)) |
| pyserial | installed automatically by `setup.py` |
| tkinter | included with Python on Windows/macOS — Linux: `sudo apt install python3-tk` |
| OS | Windows 10/11 · macOS 12+ · Ubuntu 20.04+ |

### Hardware

Any HPGL vinyl cutter connected via USB-to-serial. Tested on:
- **Seikitech SK1350** (primary target)
- Any plotter with CH340/CH341 or FTDI USB-serial chipset

**Windows driver:** [CH341SER — wch-ic.com](https://www.wch-ic.com/downloads/CH341SER_EXE.html)

---

## Installation

```bash
# 1. Clone or download the repo
git clone https://github.com/youruser/svg2plotter.git
cd svg2plotter

# 2. Run the installer
python setup.py

# 3. Use the Desktop shortcut — or run directly:
python svg2plotter.py
```

The installer will:
- Check Python version and tkinter
- Install `pyserial` via pip
- Generate `svg2plotter.ico` (pure Python, no deps)
- Create a launcher (`.bat` / `.command` / `.sh`)
- Create a Desktop shortcut with the custom icon

---

## Quick Start

1. **Connect your cutter** via USB and power it on
2. Open SVG2Plotter
3. Click **↻** to scan for serial ports — select the correct one (e.g. `COM5`)
4. Click **▶ TEST** to verify communication
5. Click **+ ADD** to load one or more SVG files
6. Arrange them on the canvas (drag, or use **⟳ AUTO**)
7. Set scale if needed: type value → **SAVE** → select SVG → **APPLY**
8. Choose cut mode: **◼ NORMAL** (opaque surfaces) or **⟺ MIRROR** (glass/window)
9. Click **✂ SEND ALL TO PLOTTER**

---

## Cut Modes

| Mode | Use case |
|---|---|
| **◼ Normal** | Vinyl applied to opaque surfaces (cars, walls, signs) — reads correctly from the front |
| **⟺ Mirror** | Vinyl applied to glass or acrylic from behind — reads correctly from the outside |

---

## HPGL Axis Mapping (SK1350)

The SK1350 uses a non-standard axis orientation:

```
HPGL X → cutter head movement  (max 1350 mm / 54000 units)
HPGL Y → paper/vinyl feed      (unlimited length)

Normal mode:  SVG X (flipped) → HPGL Y  |  SVG Y (inverted) → HPGL X
Mirror mode:  SVG X (as-is)   → HPGL Y  |  SVG Y (inverted) → HPGL X
```

Units: `1 mm = 40 HPGL units`  
Command terminator: `\x03` (ETX — required by SK1350)

---

## File Structure

```
svg2plotter/
├── svg2plotter.py   # Main application
├── setup.py         # Installer
├── README.md        # This file
└── SVG2Plotter_Manual_Utilizador.pdf   # Full user manual (PT)
```

---

## SVG Compatibility Notes

- Files **with** `width`/`height` attributes use those dimensions directly
- Files **without** `width`/`height` (e.g. exported from some tools) derive dimensions from `viewBox` at 96 dpi — aspect ratio is always preserved
- All SVG transforms are applied: `matrix()`, `translate()`, `scale()`, `rotate()`, `skewX/Y`
- Supported elements: `path`, `rect`, `circle`, `ellipse`, `line`, `polyline`, `polygon`
- Inkscape layers and groups with nested transforms are fully supported

---

## Development

Built in a single session using [Claude.ai](https://claude.ai) — an experiment in AI-assisted rapid prototyping for technical tools in a social innovation context.

> *"Vibe Coding with Claude.ai"* — the entire application, installer, and manual were developed iteratively through conversation with Claude, starting from direct serial communication tests all the way to a production-ready GUI with custom design system.

---

## License

**© 2025 David Marques · Centro de Inovação Carlos Fiolhais · CDI Portugal**

[![CC BY-NC-ND 4.0](https://licensebuttons.net/l/by-nc-nd/4.0/88x31.png)](https://creativecommons.org/licenses/by-nc-nd/4.0/)

This work is licensed under [Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International](https://creativecommons.org/licenses/by-nc-nd/4.0/).

You are free to share this work with attribution. Commercial use and derivative works are not permitted without written authorisation.
