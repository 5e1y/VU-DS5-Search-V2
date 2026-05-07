# Tokens

This folder contains the design tokens for the DS5 design system.

## Files

Load order matters — each file depends on the ones above it.

1. **`primitives.css`** — Root / primitive variables. **NEVER use these directly in designs or components.** They exist only to be referenced by semantic tokens.
2. **`semantic.css`** — Semantic tokens (`--surface-*`, `--text-icon-*`, `--interactive-*`, `--spacing-*`, `--radius-*`, `--stroke-*`, `--shadow-*`, `--effect-*`) + responsive themes applied via `[data-theme='Desktop' | 'Tablet' | 'Mobile']`. **This is what designs and components must consume.**
3. **`utilities.css`** — Drop-in utility classes mirroring Figma's Text Styles (`.normal-*`, `.medium-*`, `.medium-underline-*`, `.semi-bold-*`, `.heading-*`) and Effect Styles (`.shadow-*`, `.special-focus-*`, `.special-primary-on-picture-*`). Use these classes directly on elements instead of re-declaring font/shadow rules.

## Naming conventions

Primitives use these prefixes:

| Prefix                     | Purpose                                             |
|----------------------------|-----------------------------------------------------|
| `--primitive-*`            | Raw color values and spacing scale (px)             |
| `--letter-spacing-*`       | Raw letter-spacing values                           |
| `--font-family-*`          | Font family names                                   |
| `--font-weight-*`          | Font weight numeric → keyword map                   |
| `--screen-size-*`          | Breakpoint values                                   |
| `--font-size-*`            | Font-size scale (aliases over `--primitive-*`)      |
| `--line-height-*`          | Line-height scale (aliases over `--primitive-*`)    |
| `--paragraph-spacing-*`    | Paragraph-spacing scale (aliases over `--primitive-*`) |

> The `font-size`, `line-height` and `paragraph-spacing` scales are labelled "Aliases" by the token generator, but in the context of THIS project they are still considered **primitives** — they are numeric scales, not semantic roles. Designs should consume them through a semantic typography token (e.g. `--text-heading-lg`, to be defined).

## Rule of thumb for AI assistants

> **If you are writing CSS for a component, a screen or any design artifact and you are about to type `var(--primitive-…)`, `var(--font-size-…)`, `var(--line-height-…)`, `var(--letter-spacing-…)`, `var(--paragraph-spacing-…)`, `var(--font-family-…)`, `var(--font-weight-…)` or `var(--screen-size-…)` — STOP.**
>
> These are primitives. Use a semantic token from `semantic.css`, or a utility class from `utilities.css`, instead. If what you need does not exist, ask the user before inventing one.

## Typography rule of thumb

For body text and headings, prefer applying a utility class (`class="heading-700"`, `class="medium-400"`, …) rather than hand-rolling `font-family` / `font-size` / `line-height` / `letter-spacing` declarations. If you need a typographic combination that no utility class covers, ask first.
