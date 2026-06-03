---
name: prototype-component
description: >
  Use this skill when the user wants to prototype a single isolated UI
  component for a client — the same format used in the AI prototyping
  benchmark. Trigger phrases include "prototype d'un composant",
  "composant isolé", "teste un composant", "component prototype", "isolated
  component", "prototype a Button", "prototype a Modal". The skill
  collects component requirements, ingests client context, and produces
  a focused Lovable prompt with all variants and states.
metadata:
  version: "0.1.0"
  author: "Thirdbridge"
---

# prototype-component

## Quand cette skill se déclenche

L'utilisateur (PMPO Thirdbridge) veut prototyper **un seul composant UI** avec toutes ses variantes et états visibles côte à côte. C'est le format exact testé dans le benchmark de prototypage IA: un composant isolé, sans contexte de page, pour valider rapidement design system + interactions.

Si l'utilisateur parle d'un écran complet, utiliser `prototype-mobile-screen` ou `prototype-web-screen`. Si c'est plusieurs composants reliés en parcours, utiliser `prototype-flow`.

## Comportement attendu

### Étape 1: Confirmer le composant

Reformuler en une phrase. Exemple: « Tu veux un composant de carte produit avec image, titre, prix et bouton CTA, dans plusieurs états. C'est ça? ».

### Étape 2: Collecter les spécifications

Un seul `AskUserQuestion` avec:

1. **Catégorie de composant** — pills: « Bouton / CTA », « Carte (card) », « Formulaire (input, select, checkbox) », « Modale / Dialog », « Navigation (tabs, menu, breadcrumb) », « Feedback (toast, alert, badge) », « Liste / Item », « Autre (préciser) »
2. **Variantes à inclure** — multiSelect: « Primary », « Secondary », « Tertiary / Ghost », « Destructive », « Avec icône », « Avec image », « Compact », « Large »
3. **États à inclure** — multiSelect: « Default », « Hover », « Active / Pressed », « Focus (visible outline) », « Disabled », « Loading », « Erreur », « Succès »
4. **Plateforme** — pills: « Web (desktop + tablette) », « Mobile uniquement », « Responsive (les deux) »

Pour « Autre (préciser) », demander à l'utilisateur de décrire en texte libre.

### Étape 3: Ingérer le contexte client

Même mécanique que les autres skills, via `AskUserQuestion` multiSelect:

- Screenshots / images de référence
- Lien Figma (MCP si connecté)
- URL site déployé
- Repo Git local
- Aucun contexte

Pour un composant, l'extraction se concentre sur:
- 2 à 4 couleurs critiques (primary, accent, neutral text, surface)
- Famille de typographie body
- Border-radius, ombres, transitions si visibles
- Spacing / padding pattern (4px, 8px, 16px scale?)

### Étape 4: Assembler le prompt Lovable

Template:

```
Build an isolated UI component prototype.

STACK
React 18 + TypeScript + Tailwind CSS. Single component file plus a
showcase page that renders all variants and states side-by-side.

GOAL
Build a [Q1, ex. "primary action button"] component with the following
variants and states, all displayed on a single showcase page for visual
review.

VARIANTS
[Liste de Q2, chaque variante en bullet point. Ex:
- Primary (filled)
- Secondary (outlined)
- Tertiary (ghost / text only)
- Destructive (red filled)]

STATES (per variant)
[Liste de Q3 en bullet:
- Default
- Hover
- Focus (visible 2px outline offset)
- Disabled (opacity 0.5, no pointer events)
- Loading (spinner replaces label)]

LAYOUT
Showcase page with a grid: variants as columns, states as rows. Each
cell labeled with variant + state name. Padding 24px between cells.
Light gray background to make whites visible. Sticky header with the
component name.

PLATFORM
[Q4 — adapter taille / padding]

REQUIREMENTS
- TypeScript: typed props interface exported
- Accessibility: keyboard navigation, ARIA attributes, focus-visible
- Transitions: 150ms ease on hover/focus changes
- No external deps beyond React and Tailwind

BRAND CONTEXT
[Bloc de l'étape 3. Fallback: "Neutral modern: slate-900 text, white
surface, indigo-600 primary, 8px radius, system font."]

DELIVERABLES
- src/components/[ComponentName].tsx — the component itself
- src/App.tsx — the showcase page rendering everything
- Lovable share link ready
```

### Étape 5: Livrer au PMPO

Même format de livraison: prompt dans un bloc de code, lien Lovable, instructions.

## Anti-patterns

- Ne pas mélanger plusieurs composants dans la même skill: un composant = un prompt. Si le PMPO veut un système complet (button + input + card), produire un prompt par composant ou rediriger vers `prototype-web-screen` pour un design system showcase.
- Toujours montrer toutes les variantes et états côte à côte: c'est le format qui a gagné le benchmark.
- Prompt final en anglais, max 220 mots.
