# azs-cad — PCB design in the browser

[🇫🇷 Français](LISEZMOI.md) · **🇬🇧 English**

A single HTML page, `azs-cad.html`: library, schematic and board of one project, in tabs. Plain HTML/CSS/JavaScript, no dependency, works offline. In the spirit of Eagle 4.01 without cloning it. The interface is in French or English depending on the browser.

![Board](screenshot-board.png)

## Features

- **Library**: standard THT/SMD footprints (DIP, SOIC, QFN, TQFP, 0402–2512…), pin names, vendor footprint import (`.zip` / `.lbr`).
- **Schematic**: R, C, D, L, PAD, project ICs, buses, VCC+/GND, 50 Ω lines.
- **Board**: 2, 4 or 6 layers, GND and VCC planes, manual 45° routing, automatic vias, clearance check, true-size silkscreen.
- **50 Ω RF patches** (400 to 3000 MHz), computed copper: Wilkinson splitter, T splitter with 2/4/8 outputs, directional coupler, low-pass and band-pass filters, patch antenna, λ/4 monopole.
- **Outputs**: Gerber RS-274X + Excellon as a zip with preview, `.bom.csv` bill of materials.
- **Files**: `<project>.lib`, `<project>.sch.dxf` and `<project>.brd.dxf` (AutoCAD R12). Ctrl+S saves the project.

![Schematic](screenshot-schematic.png)

![Patch](screenshot-patch.png)

## License

[CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/deed.en): free for non-commercial use, with credit "azs-cad — Eric PERRET — https://github.com/ericperret/pcb-cad". Commercial use requires written permission: eperret@azs.fr. See `LICENSE`.

© Eric PERRET 2026 · eperret@azs.fr · www.azs.fr
