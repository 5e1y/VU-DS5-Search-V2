# CLAUDE.md — Repo de prototypage DS5

Ce repo génère des **prototypes Vue rapides et interactifs** qui suivent le design
system DS5 de Vente-Unique. Les composants ici sont un **miroir de prototypage**
(fidélité, pas production) : temporaires, remplacés par les specs officielles du
Storybook à mesure qu'elles arrivent.

Avant de composer un écran, **lis `components.manifest.json`** : il liste les
composants disponibles et leur API. Ne suppose jamais qu'un composant existe — vérifie.

---

## Loi des tokens (non négociable)

Le système a trois couches : `primitives.css` → `semantic.css` → `utilities.css`.

1. **Couleurs, espacements, radius, stroke, ombres, surfaces, états interactifs**
   → consomme uniquement les **tokens sémantiques** :
   `--surface-*`, `--text-icon-*`, `--interactive-*`, `--spacing-*`,
   `--radius-*`, `--stroke-*`, `--shadow-*`, `--effect-*`.
2. **Typographie** → utilise les **classes utilitaires** :
   `heading-*`, `normal-*`, `medium-*`, `semi-bold-*`, `medium-underline-*`.
   N'écris jamais `font-family` / `font-size` / `line-height` à la main.
3. **INTERDIT** : référencer un `--primitive-*` (ou un `--font-size-*`,
   `--line-height-*`, `--letter-spacing-*`) directement. Écrire un hex, un rgba
   ou un px en dur. Seules exceptions tolérées : durées de transition (ex. `150ms`)
   et zéros structurels (`0`, `border-radius: 0`).

## Icônes

Toujours via le composant `iconMain` (`<iconMain :icon="nom" :size="..." />`).
**Jamais** d'emoji, jamais de SVG/image ad hoc en guise d'icône. Le nom d'icône
doit exister dans le set ; en cas de doute, vérifie le manifest plutôt qu'inventer.

## Composants

- N'utilise que des composants présents dans `components.manifest.json`.
- Ne ré-implémente pas un composant existant inline ; ne le restyle pas par-dessus
  (pas d'override de ses classes/tokens).
- Si un composant nécessaire **manque**, arrête-toi et signale-le — n'improvise pas
  un sosie. Un trou dans le DS est une information utile, pas un problème à masquer.

## Responsive

Le thème s'applique via `data-theme="Desktop" | "Tablet" | "Mobile"` sur la racine.
Par défaut `Desktop`. Utilise `--responsive-margin` et les tokens `--responsive-*`
pour les marges d'écran.

## Polices

Poppins (corps) et Recoleta Alt (titres) doivent être chargées dans le repo.
Recoleta Alt est sous licence — sans elle, les titres tombent en fallback et la
fidélité casse.

---

## Conventions d'écriture d'un composant (si tu en crées un)

Calque-toi sur les composants existants :

- `<script setup lang="ts">`, Composition API.
- Props typées via `defineProps<{...}>()` + `withDefaults`.
- Variantes = unions de littéraux string (`type ButtonType = 'primary' | ...`).
- Classes BEM : `nom`, `nom--variante`, `nom__partie`.
- `<style scoped>` consommant **uniquement** des tokens sémantiques.
- Patron polymorphe quand pertinent (`<component :is="tag">` : `a` si `href`, sinon `button`).
- Gère toujours `disabled` (button **et** lien via `aria-disabled`) et `:focus-visible`.

---

## Auto-vérification avant de rendre (garde-fou hallucinations)

- [ ] Aucun `--primitive-*`, aucun hex / rgba / px en dur.
- [ ] Typo via classes utilitaires, pas de déclaration de police à la main.
- [ ] Aucune icône emoji ; icônes via `iconMain`.
- [ ] Aucun composant hors `components.manifest.json`.
- [ ] `data-theme` posé sur la racine.
