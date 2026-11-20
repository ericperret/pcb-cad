# azs-cad

A printed circuit board CAD that runs in a web browser: **one HTML page, `azs-cad.html`**, plain HTML, CSS and JavaScript, no library, no dependency, works offline. Inspired by the spirit of Eagle 4.01 (library, schematic, board), not a clone. The interface is French or English depending on the browser. Current version: **1.73.1**.

![Schematic](azs-schematic.png)

![Board](azs-board.png)

![Gerber preview](azs-gerber.png)

## Modules

One project is open at a time and shared in memory by the three modules (nothing is stored in the browser).

- **Library**: common THT and SMD footprints, pin names, standard footprints after IPC-7351 (SSOP, TSSOP, MSOP, TQFP up to 208 pins, QFN up to 64 pins, DFN/SON, BGA, FBGA, uBGA/CSP, WLCSP, SOT, D2PAK, crystals, SMD 0201 to 1812, tantalum A to D; `internal.lib` loads them all), import of manufacturer footprints (`.zip` or `.lbr`).
- **Schematic**: passives, integrated circuits (logic symbol), connectors, buses (Ctrl+click on several wires ▸ Bus, editable name, buses across sheets), supplies (right click on a pin: grounds, common voltages or free name), 50 ohm lines, multifunction pin choice. Automatic, hidden grid at the pin pitch of the finest part: when placing, moving or rotating, pin 1 lands on the grid. Smooth editing even on a large schematic (one drawing per frame, only what is visible is drawn, provisional wires while dragging). Optimize: placement, 45 degree wires, spaced wires, net labels instead of overlong wires, supply ranking, **decoupling review**.
- **Board**: 2 to 8 layers, ground and supply planes, manual routing at 45 degrees, automatic vias, via in pad for ball grid packages, clearance checks, silkscreen, optimization and automatic routing.

## Board

- **Stack-up**: one ground plane below, one plane per secondary supply (same voltages grouped on one layer), signals everywhere. Panel *Supply planes*: per net, auto / no plane / chosen inner layer.
- **Fabrication minimums** from PCBCart + 33 %; only track width and via drill are set, the copper diameter follows.
- **Route the ratsnest**: automatic routing by priority (50 ohm lines, signals, supplies, grounds), clockwise spiral from the densest IC, top then bottom pass. A net that has a plane is never drawn as tracks: its pins reach the plane through their via.
- **Re-route**, three modes (50 ohm lines, patches, planes and filled faces excepted), progress shown in yellow, Esc to stop, a single Ctrl+Z:
  - *shortest*: every track is undone and redrawn; the new trace is kept only if its cost (length + vias) does not rise, or if it runs less often between the pins of a fine-pitch part; pin by pin, segment by segment, step by step (Space, arrows, Esc) with the decision and its reason displayed. A **clearance check** ends it: a track or via too close to foreign copper or to the edge, or lying between two pins, goes back to the ratsnest;
  - *at any cost*: every net is undone and redrawn, supplies routed as tracks first then from shortest to longest, kept even if longer; length and via report before / after;
  - *negotiated* (PathFinder method): nets compete for room at a congestion cost that rises at each iteration; order no longer decides; the result is kept only without conflict, with connectivity intact and a lower total cost, then refined net by net.
- **Router cost model**: no penalty for deeper layers; only crossing a plane layer stays expensive; a via counts as a layer change (1.5 mm by default); the via of an SMD pin sits at the exact centre of its pad, never stuck beside it; pad too thin for a via: the via moves away along the pad axis; vias between the pads of one part or too close to a foreign pad are penalised.
- **Vias**: a via already in the pad is reused; duplicates and useless layer hops (short track between two vias, same layer on both sides) are removed by Optimize ▸ Vias and at the end of re-routing.
- **Fine-pitch parts** (pitch 1.27 mm or less, SOIC included): no track and no via between two neighbouring pins; two vias and a pass underneath or beside are preferred.
- **Routing weights** (Optimize): distance, power layer, via (mm per layer change), crossing under a 50 ohm line, drawing in a power layer, crossing between fine pins; saved with the board.
- **Grid and mesh**: board grid always automatic (finest IC pitch); routing mesh independent, dividing the ball pitch (via in every ball); under a ball grid package, inner layers keep their copper bridges between vias.
- **Group selection**: Ctrl + rectangle(s), release Ctrl: parts, tracks, vias and texts of the area follow the mouse, click to drop; tracks cut by the area edge go back to the ratsnest.
- **Optimize**: vias, 45 degree corners, parallel tracks spaced, tracks centered between neighbours (a via follows the track, or its entry is shifted at 45 degrees), LEDs / resistors / coupling capacitors moved close to their pins.
- **Right click on a track**: width, centering, **Round** (one single arc; on an integrated circuit pad, straight exit along the pad axis, then the arc).
- **Outline**: format of choice, margin, square, convex or concave corners; *Recenter* gives equal margins top / bottom and left / right.
- **Layers**: *Empty* an inner layer re-routes its nets elsewhere and lists what remains.
- **Irrigation bridge**: a supply plane cut in two islands is fed again by a 1 mm x 5 mm line on the inner signal layers with four DIL-like vias and a silkscreen text (IRRIGATION net).
- **Decoupling**: *Review* proposes, per IC supply pin, one 100 nF and one bulk capacitor, and deletes the redundant ones (applied to the schematic too). *Place* puts each capacitor in the nearest free spot around the pin it serves, in a spiral.

## 50 ohm RF

- Line width follows the substrate thickness.
- Right click on a 50 ohm line: length, **Round** (one single arc over the whole full-width line); a line shorter than 1 cm gets the 50 ohm width offered first.
- The router lets other signals cross under a thick 50 ohm line only as a last resort: inner layer, at 90 degrees, straight, fixed length, counted in the report.
- Patterns from 400 to 3000 MHz: Wilkinson 2-way divider, T divider 2/4/8 outputs, directional coupler 15/20/30 dB, low-pass and band-pass filters, rectangular patch antenna, quarter-wave monopole.

## Outputs

- Gerber RS-274X and Excellon in a zip with preview (unfed zones shown). The export is refused while a silkscreen text lies on a pad.
- Bill of materials `.xls` and `.csv` with the columns of the sample BOM.
- Project: `<project>.azs.zip` containing `<project>.lib`, `<project>.sch.dxf` and `<project>.brd.dxf` (DXF R12). Ctrl+S saves it. The board is never changed by itself: *Update the board* is offered when the schematic differs.

## Use

1. Library tab: create a project, open a footprint library, tick the components.
2. Schematic tab: place, wire, optimize.
3. Board tab: update from the schematic, route, optimize, export.

Open an existing project by choosing its `.azs.zip` (an old project: `.lib` with its `.sch.dxf` and `.brd.dxf`).

Example of board optimization on `tvp5150_essai`: track length from 1707 to 1156 mm, vias from 41 to 13.

## License

[CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/): free non commercial use with attribution to Eric PERRET. Any commercial use requires the written authorization of the author (eperret@azs.fr).
