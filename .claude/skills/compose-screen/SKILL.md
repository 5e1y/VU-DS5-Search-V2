---
name: compose-screen
description: >-
  Compose un écran ou un prototype Vue à partir du design system DS5. À utiliser
  dès que l'utilisateur demande de générer, maquetter ou assembler un écran, une
  page, un parcours ou une interface (PDP, PLP, home, formulaire, tunnel...) qui
  doit suivre le design system. Se déclenche aussi sur "génère-moi un écran",
  "maquette une page", "prototype ce parcours".
---

# Composer un écran DS5

Objectif : produire un écran Vue fidèle au design system, **sans inventer** de
style ni de composant. Les règles de fond (tokens, icônes, polices, conventions)
sont dans `CLAUDE.md` — ne les redécris pas, applique-les.

## Procédure

1. **Cadrer.** Identifier le type d'écran et le viewport cible
   (Desktop / Tablet / Mobile). Poser `data-theme` correspondant sur la racine.

2. **Inventorier.** Lire `components.manifest.json`. Lister les composants dont
   l'écran a besoin et vérifier qu'ils existent. Pour chacun, lire ses `props`,
   `options`, `usage.doNot` et `examples`.

3. **Détecter les trous.** Si un composant nécessaire **manque** du manifest :
   ne pas improviser un sosie. Le signaler à l'utilisateur et proposer soit une
   alternative existante, soit de coder d'abord le composant manquant (selon les
   conventions de `CLAUDE.md`).

4. **Composer.** Assembler l'écran avec les composants du manifest. Layout, marges
   et espacements via les tokens sémantiques (`--spacing-*`, `--responsive-*`).
   Textes via les classes utilitaires (`heading-*`, `medium-*`...). Icônes via
   `iconMain` uniquement.

5. **Rendre interactif (si demandé).** Câbler l'état local et la navigation pour
   un parcours cliquable. Rester sur les events/props documentés des composants.

6. **Auto-vérifier** avant de livrer, avec la checklist de `CLAUDE.md` :
   pas de `--primitive-*`, pas de hex/px en dur, pas d'emoji-icône, aucun composant
   hors manifest, `data-theme` posé, typo en classes utilitaires.

## Sortie attendue

Un (ou plusieurs) fichier(s) `.vue` autonomes, importables, qui compilent contre
les composants du repo. Pas de CSS global réinventé : tout passe par les tokens
et les composants existants.
