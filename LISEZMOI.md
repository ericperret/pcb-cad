# azs-cad — CAO de circuits imprimés dans le navigateur

**🇫🇷 Français** · [🇬🇧 English](README.md)

Une seule page HTML, `azs-cad.html` : bibliothèque, schéma et circuit imprimé d'un même projet, en onglets. HTML/CSS/JavaScript natif, aucune dépendance, fonctionne hors ligne. Esprit Eagle 4.01, sans en être un clone. Interface en français ou en anglais selon le navigateur.

![Circuit](capture-circuit.png)

## Fonctions

- **Bibliothèque** : gabarits normalisés DIL/CMS (DIP, SOIC, QFN, TQFP, 0402–2512…), nom des pattes, import de gabarits fournisseur `.zip` / `.lbr`.
- **Schéma** : R, C, D, L, PAD, circuits du projet, bus, VCC+/GND, lignes 50 Ω.
- **Circuit** : 2, 4 ou 6 couches, plans GND et VCC, routage manuel 45°, vias automatiques, contrôle d'isolement, sérigraphie au vrai.
- **Patchs RF 50 Ω** (400 à 3000 MHz), cuivre calculé : diviseur Wilkinson, diviseur en T 2/4/8 sorties, coupleur directionnel, filtres passe-bas et passe-bande, antenne patch, monopôle λ/4.
- **Sorties** : Gerber RS-274X + Excellon en zip avec aperçu, nomenclature `.bom.csv`.
- **Fichiers** : `<projet>.lib`, `<projet>.sch.dxf` et `<projet>.brd.dxf` (AutoCAD R12). Ctrl+S enregistre le projet.

![Schéma](capture-schema.png)

![Patch](capture-patch.png)

## Licence

[CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/deed.fr) : usage non commercial libre, avec mention « azs-cad — Eric PERRET — https://github.com/ericperret/pcb-cad ». Usage commercial sur autorisation écrite : eperret@azs.fr. Voir `LICENSE`.

© Eric PERRET 2026 · eperret@azs.fr · www.azs.fr
