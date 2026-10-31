# azs-cad — CAO de circuits imprimés / PCB design in the browser

**FR** — Bibliothèque de composants, saisie de schéma et circuit imprimé multicouche, en trois pages HTML autonomes (HTML/CSS/JavaScript natif, aucune dépendance, fonctionne hors ligne). Esprit Eagle 4.01, sans en être un clone : routage manuel, 4 couches de cuivre, export Gerber RS-274X et Excellon.

**EN** — Component library, schematic capture and multilayer PCB, in three standalone HTML pages (plain HTML/CSS/JavaScript, no dependency, works offline). In the spirit of Eagle 4.01 without cloning it: manual routing, 4 copper layers, Gerber RS-274X and Excellon export.

L'interface s'affiche en français si le navigateur est en français, en anglais sinon. / The interface is in French when the browser is set to French, in English otherwise.

---

## Fichiers / Files

| Fichier / File | Version | Rôle / Role |
|---|---|---|
| `bibliotheque.html` | 0.4.0 | Bibliothèque : composants, gabarits, nom des pattes / Library: components, footprints, pin names |
| `schema.html` | 0.5.0 | Saisie de schéma / Schematic capture |
| `board.html` | 0.8.0 | Circuit imprimé : placement, routage, export / Board: placement, routing, export |
| `README.md` | | Documentation |
| `LICENSE` | | CC BY-NC 4.0 |

Les trois pages doivent être dans le **même dossier** et ouvertes dans le **même navigateur** (Chrome ou Edge conseillés). / The three pages must sit in the **same folder** and be opened in the **same browser** (Chrome or Edge recommended).

---

# Français

## Démarrage

1. Ouvrir `bibliotheque.html`, créer ou ouvrir une bibliothèque `.lib`, l'enregistrer.
2. Ouvrir `schema.html`, charger la bibliothèque, saisir le schéma.
3. Bouton **Circuit ↗** : `board.html` s'ouvre sur le même projet. Le circuit suit le schéma en direct, sans enregistrement ni rechargement.
4. Placer, router, choisir le contour, **Export Gerber** : aperçu, puis téléchargement d'un zip.

## Bibliothèque (`bibliotheque.html`)

- Double vue : **SCH** (fond blanc, pattes vertes) et **CIRCUIT** (fond noir, cuivre rouge), zoom et déplacement partagés.
- **+ Nouveau composant** : famille normalisée et nombre de pattes, puis nom (ex. `NE555`). Familles : DIP étroit/large, SIL, HE10, bornier, axial, radial, TO-92/126/220, HC-49, SOIC, TSSOP, PLCC, TQFP, QFN, CMS 0402 à 2512, diodes CMS, SOT, DPAK.
- Nom des pattes : clic sur la patte (Tab = patte suivante), clic droit = liste (VCC+, GND…), ou tableau Patte | Nom.
- **Retoucher** : sélection, poignées, déplacement, rotation (R), suppression (Suppr), type DIL/CMS changé sur place. **Dessiner** : pads DIL/CMS, trait, rectangle, arc, cercle, point, texte.
- Le vernis n'est pas dessiné : il découle des pastilles.

Format `.lib` (texte, mm, degrés, une instruction par ligne, `#` = commentaire) :

```
PACKAGE DIP8
PAD 1 -3.81 -3.81 0.8 1.3 OVALE 90      # nom x y perçage diamètre ROND|CARRE|OVALE rotation
SMD 1 -0.95 0 0.6 1.0 0                  # nom x y dx dy rotation
TRAIT x1 y1 x2 y2 épaisseur
RECT x1 y1 x2 y2 épaisseur
ARC cx cy r angle1 angle2 épaisseur      # sens trigonométrique
CERCLE cx cy r épaisseur
POINT x y r
FIN
COMPOSANT NE555 DIP8 SOIC8               # nom puis gabarits possibles
GND => 1
VCC+ => 8
FIN
```

## Schéma (`schema.html`)

- Palette : **R** (zigzag), **C** (deux plaques, polarisé « + » au-delà de 1 µF, forçable), **D** (patte 1 = cathode), **L**, **PAD** (une patte), **IC** (dessiné comme dans la bibliothèque). Repères automatiques R1, C1, D1, L1, P1, IC1 ; libellé `IC1.NE555`, `R1.10K`.
- Clic dans le vide : repose le dernier composant, même valeur.
- Liaison : clic patte → patte. Clic droit sur une patte : **VCC+** ou **GND**.
- Groupes : Ctrl+clic / Maj+clic sur des pattes ; deux groupes de même taille = **bus** (peignes et tronc gras marqué du nombre), appairage par numéro de patte.
- Fil sélectionné : carré = déplacer un coude, rond au milieu = nouveau coude, double-clic = effacer. Type **Électrique** ou **Ligne 50 Ω** (orange pointillé). Tracé automatique orthogonal.
- Croisement d'équipotentielles différentes : encoche ; même équipotentielle : point.
- R : rotation · Suppr : supprimer · Ctrl+Z / Ctrl+Y · Ctrl+S.

## Circuit (`board.html`)

### Placement
- Import du schéma (ou lien **Schéma ↗ / Circuit ↗**) : composants pré-placés selon le schéma, écartés juste assez pour ne pas se chevaucher.
- Gabarit choisi dans la fiche : ceux de la bibliothèque, puis les **normalisés** générés (mêmes familles que la bibliothèque, pastille seule traversante ou CMS). Pour les passifs, le dernier gabarit choisi est repris.
- R : rotation · F : dessus / dessous (le dessous s'affiche en bleu, vu de dessus).
- Le texte `IC1.NE555` est collé au composant ; Maj + glisser le décale, double-clic le recentre.
- **Texte** (T) : texte libre de sérigraphie, hauteur en mm, face au choix.
- Un composant déplacé, tourné, retourné ou changé de gabarit perd son cuivre : ses liaisons repartent en chevelu.

### Routage
- **Router**, puis clic sur une pastille : départ sur la couche de la face du composant (cuivre 1 dessus, cuivre 4 dessous).
- Chaque clic pose un coude droit puis à 45°. Touches **1 à 4** ou boutons colorés : changement de couche, via posée automatiquement.
- Fin : clic sur une pastille, une piste ou une via de la même équipotentielle, sur n'importe quelle couche (via posée d'office si besoin) ; clic droit ou Échap : arrêt en gardant les segments ; **double-clic : annule le tracé en cours**.
- Hors tracé : double-clic sur une piste ou une via = dé-routage (retour au chevelu jaune pointillé).
- Clic sur un segment : sélection, glisser = déplacer, Suppr = supprimer ; clic sur l'extrémité libre d'un segment sélectionné : prolonger. Clic droit sur une piste : largeur standard (0,2 à 2,54 mm) ou saisie.
- Le chevelu jaune pointillé suit le tracé ; les lignes 50 Ω sont en orange.
- Isolement : un passage trop près du cuivre d'une autre équipotentielle est entouré d'un cercle mauve avec la distance mesurée ; la pose n'est jamais refusée.
- Compteur permanent des liaisons restant à router dans la barre d'état.

### Réglages de fabrication
| Réglage | Défaut | Minimum (PCBCart standard × 2) |
|---|---|---|
| Largeur de piste | 1 mm, puis la dernière utilisée | 0,152 mm |
| Isolement | 0,2 mm | 0,152 mm |
| Via (perçage / Ø) | 0,8 / 1,6 mm | perçage 0,304 mm, couronne 0,152 mm |
| Substrat | FR4 1,6 mm (ou 0,8), εr 4,5, cuivre 35 µm | |

Largeur 50 Ω (microruban, IPC-2141) : `w = (5,98·h / exp(50·√(εr+1,41) / 87) − t) / 0,8`, soit 2,914 mm en 1,6 mm.

### Contour et export
- Contour : **Auto** (carte de crédit 85,6 × 54, sinon Europe 160 × 100, sinon taille juste au-dessus), format imposé ou **Personnalisé** ; 5 mm de marge, centré sur le projet.
- 4 mires d'alignement à 3 mm des coins (cuivre Ø 2, vernis Ø 2,4, sérigraphie Ø 3, trou Ø 1) et « **AZS** » sur toutes les couches pour contrôler le sens.
- **Export Gerber** : aperçu relu depuis les fichiers produits (miniatures vues de dessus, loupe sur « AZS », vue agrandie avec couches superposables, transparence, mesure), puis **Télécharger le zip** `<nom>-gerber.zip` :

| Fichier | Contenu |
|---|---|
| `-cuivre1.gtl` | Cuivre 1 (dessus) |
| `-cuivre2.g2l`, `-cuivre3.g3l` | Cuivres internes, s'ils portent une piste |
| `-cuivre4.gbl` | Cuivre 4 (dessous) |
| `-vernis-dessus.gts`, `-vernis-dessous.gbs` | Ouvertures de vernis (marge 0,1 mm) |
| `-serigraphie-dessus.gto`, `-serigraphie-dessous.gbo` | Sérigraphie (police au trait, majuscules) |
| `-contour.gko` | Contour |
| `-percage.drl` | Perçage Excellon (mm) |

## Lien schéma ↔ circuit
Chaque page publie son état à chaque changement dans le stockage local du navigateur ; l'autre page le suit sans action. Ce qui n'a pas changé reste en place et routé ; un composant ajouté arrive à droite de la carte ; un composant supprimé part avec son cuivre ; une liaison modifiée fait sauter les pistes de son équipotentielle. **Enregistrer** produit les fichiers sur disque.

## Formats
- Schéma `.sch.dxf` et circuit `.brd.dxf` : **AutoCAD R12 (AC1009)** multicalque, lisibles par tout logiciel DAO ; données azs-cad en XDATA (APPID `AZSCAD`, les fichiers `AIGLECAD` des premières versions restent lus).
- Calques du circuit : `CUIVRE_1` à `CUIVRE_4`, `VERNIS_DESSUS`, `VERNIS_DESSOUS`, `TEXTE_DESSUS`, `TEXTE_DESSOUS`, `PERCAGE`, `CHEVELU`, `AZSCAD`.
- Bibliothèque `.lib` : texte (voir plus haut).

## Limites
Pas d'autoroutage, pas de plans de masse, vias traversantes uniquement, pas de couche restrict/keepout. Le lien schéma ↔ circuit exige le même navigateur.

---

# English

## Getting started

1. Open `bibliotheque.html`, create or open a `.lib` library, save it.
2. Open `schema.html`, load the library, draw the schematic.
3. **Board ↗** button: `board.html` opens on the same project. The board follows the schematic live, with no saving or reloading.
4. Place, route, pick the outline, **Export Gerber**: preview, then download a zip.

## Library (`bibliotheque.html`)

- Two views: **SCH** (white background, green pins) and **BOARD** (black background, red copper), shared zoom and pan.
- **+ New component**: standard family and pin count, then name (e.g. `NE555`). Families: DIP narrow/wide, SIL, HE10, screw terminal, axial, radial, TO-92/126/220, HC-49, SOIC, TSSOP, PLCC, TQFP, QFN, SMD 0402 to 2512, SMD diodes, SOT, DPAK.
- Pin names: click the pin (Tab = next pin), right-click = list (VCC+, GND…), or the Pin | Name table.
- **Edit**: selection, handles, move, rotate (R), delete (Del), THT/SMD type changed in place. **Draw**: THT/SMD pads, line, rectangle, arc, circle, dot, text.
- Solder mask is not drawn: it derives from the pads.
- `.lib` format: see the French section above (keywords stay in French: `PACKAGE`, `PAD`, `SMD`, `TRAIT`, `RECT`, `ARC`, `CERCLE`, `POINT`, `FIN`, `COMPOSANT`, `function => pins`).

## Schematic (`schema.html`)

- Palette: **R**, **C** (polarized "+" above 1 µF, can be forced), **D** (pin 1 = cathode), **L**, **PAD** (one pin), **IC** (drawn as in the library). Automatic references R1, C1, D1, L1, P1, IC1; labels `IC1.NE555`, `R1.10K`.
- Click in empty space: places the last component again, same value.
- Connection: click pin → pin. Right-click a pin: **VCC+** or **GND**.
- Groups: Ctrl+click / Shift+click on pins; two groups of the same size make a **bus** (combs and a thick trunk labeled with the count), paired by pin number.
- Selected wire: square = move a bend, middle circle = new bend, double-click = delete. Type **Electrical** or **50 Ω line** (dashed orange). Automatic orthogonal path.
- Crossing of different nets: hop; same net: junction dot.
- R: rotate · Del: delete · Ctrl+Z / Ctrl+Y · Ctrl+S.

## Board (`board.html`)

### Placement
- Schematic import (or the **Schematic ↗ / Board ↗** link): components pre-placed as in the schematic, spread just enough not to overlap.
- Footprint picked in the side panel: library footprints first, then generated **standard** ones (same families as the library, single THT or SMD pad). Passives reuse the last footprint chosen for their type.
- R: rotate · F: top / bottom (bottom shown in blue, seen from the top).
- The `IC1.NE555` label is glued to the component; Shift + drag offsets it, double-click recenters it.
- **Text** (T): free silkscreen text, height in mm, side of your choice.
- A component that is moved, rotated, flipped or given another footprint loses its copper: its connections go back to the ratsnest.

### Routing
- **Route**, then click a pad: the trace starts on the component's side layer (copper 1 top, copper 4 bottom).
- Each click lays a straight then 45° bend. Keys **1 to 4** or the colored buttons: layer change, via placed automatically.
- End: click a pad, track or via of the same net, on any layer (via added when needed); right-click or Esc: stop and keep the segments; **double-click: cancels the current trace**.
- Outside a trace: double-click a track or via = unroute (back to the dashed yellow ratsnest).
- Click a segment: select, drag = move, Del = delete; click the free end of a selected segment: extend. Right-click a track: standard width (0.2 to 2.54 mm) or typed value.
- The dashed yellow ratsnest follows the trace; 50 Ω lines are orange.
- Clearance: passing too close to copper of another net is circled in purple with the measured distance; placement is never refused.
- Permanent counter of connections left to route in the status bar.

### Manufacturing settings
| Setting | Default | Minimum (PCBCart standard × 2) |
|---|---|---|
| Track width | 1 mm, then the last one used | 0.152 mm |
| Clearance | 0.2 mm | 0.152 mm |
| Via (drill / Ø) | 0.8 / 1.6 mm | drill 0.304 mm, annular ring 0.152 mm |
| Substrate | FR4 1.6 mm (or 0.8), εr 4.5, 35 µm copper | |

50 Ω width (microstrip, IPC-2141): `w = (5.98·h / exp(50·√(εr+1.41) / 87) − t) / 0.8`, i.e. 2.914 mm on 1.6 mm.

### Outline and export
- Outline: **Auto** (credit card 85.6 × 54, else Europe 160 × 100, else the next size up), a forced format or **Custom**; 5 mm margin, centered on the project.
- 4 alignment marks 3 mm from the corners (copper Ø 2, mask Ø 2.4, silkscreen Ø 3, hole Ø 1) and "**AZS**" on every layer to check orientation.
- **Export Gerber**: preview read back from the generated files (top-view thumbnails, "AZS" magnifier, enlarged view with stackable layers, transparency, measurement), then **Download zip** `<name>-gerber.zip` (file list in the French section).

## Schematic ↔ board link
Each page publishes its state to the browser's local storage on every change; the other page follows with no action. Unchanged parts stay placed and routed; an added component appears to the right of the board; a deleted component leaves with its copper; a modified connection drops the tracks of its net. **Save** writes the files to disk.

## Formats
- Schematic `.sch.dxf` and board `.brd.dxf`: multilayer **AutoCAD R12 (AC1009)**, readable by any CAD software; azs-cad data in XDATA (APPID `AZSCAD`; `AIGLECAD` files from early versions are still read).
- Library `.lib`: plain text.

## Limitations
No autorouter, no ground planes, through-hole vias only, no restrict/keepout layer. The schematic ↔ board link needs the same browser.

---

## Licence / License

**FR** — Libre pour un usage non commercial, sous licence [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/deed.fr), avec mention obligatoire de la source et de l'auteur : « azs-cad — Eric PERRET — https://github.com/ericperret/azs-cad ». Toute utilisation commerciale est interdite sans l'autorisation écrite de l'auteur, à demander par courriel : eperret@azs.fr. Voir `LICENSE`.

**EN** — Free for non-commercial use under the [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/deed.en) license, with mandatory credit to the source and author: "azs-cad — Eric PERRET — https://github.com/ericperret/azs-cad". Any commercial use is forbidden without the author's written permission, requested by e-mail: eperret@azs.fr. See `LICENSE`.

## Auteur / Author

© Eric PERRET 2026 · eperret@azs.fr · www.azs.fr
