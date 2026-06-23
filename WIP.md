# WIP — Prototypes en cours

Page d'accueil (GitHub Pages) listant les tests de design en cours. Chaque
bouton mène à un prototype. Ajouter une entrée dans `tests` de `src/views/Home.vue`
pour publier un nouveau test.

## Tests

- **Recherche V2** (`/recherche-v2`) — overlay de recherche : ouverture depuis la
  barre du header (morph), état par défaut (récentes / catégories / produits) et
  état saisie (suggestions d'autocomplétion). Desktop + plein écran mobile.

## Déploiement

GitHub Pages via `.github/workflows/pages.yml` (build Vite à chaque push sur `main`).
URL : https://5e1y.github.io/Vente-unique-DS5/
