---
description: Prototype a single isolated UI component with all variants and states visible side-by-side. Collects requirements, ingests client context, and outputs a focused Lovable prompt.
when_to_use: >
  Use when the user wants to prototype a single isolated component
  (button, card, form input, modal, navigation, feedback element).
  Trigger phrases: "prototype d'un composant", "composant isolé", "teste
  un composant", "component prototype", "isolated component", "prototype
  a Button", "prototype a Modal". For a full screen use prototype-mobile-
  screen or prototype-web-screen; for multiple connected screens use
  prototype-flow.
---

Format inspiré du benchmark de prototypage IA: un composant isolé, sans contexte de page, pour valider rapidement design system + interactions.

## 1. Confirmer le composant

Reformuler en une phrase. Ex: « Tu veux un composant de carte produit avec image, titre, prix et bouton CTA, dans plusieurs états. C'est ça? ».

## 2. Collecter les specs

Un seul `AskUserQuestion`:

1. **Catégorie** — pills: « Bouton / CTA », « Carte », « Formulaire (input, select, checkbox) », « Modale / Dialog », « Navigation (tabs, menu, breadcrumb) », « Feedback (toast, alert, badge) », « Liste / Item », « Autre (préciser) »
2. **Variantes** — multiSelect: « Primary », « Secondary », « Tertiary / Ghost », « Destructive », « Avec icône », « Avec image », « Compact », « Large »
3. **États** — multiSelect: « Default », « Hover », « Active / Pressed », « Focus », « Disabled », « Loading », « Erreur », « Succès »
4. **Plateforme** — pills: « Web (desktop + tablette) », « Mobile uniquement », « Responsive »

Pour « Autre », demander une description texte libre.

## 3. Ingérer le contexte client

Même mécanique que les autres skills (screenshots / Figma / URL / repo / aucun).

Extraction focalisée:
- 2 à 4 couleurs critiques (primary, accent, neutral text, surface)
- Typographie body
- Border-radius, ombres, transitions si visibles
- Spacing / padding (échelle 4/8/16?)

## 4. Assembler le prompt Lovable

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
[Liste Q2 en bullets. Ex:
- Primary (filled)
- Secondary (outlined)
- Tertiary (ghost / text only)
- Destructive (red filled)]

STATES (per variant)
[Liste Q3 en bullets:
- Default
- Hover
- Focus (visible 2px outline offset)
- Disabled (opacity 0.5, no pointer events)
- Loading (spinner replaces label)]

LAYOUT
Showcase page with a grid: variants as columns, states as rows. Each cell
labeled with variant + state name. Padding 24px between cells. Light
gray background to make whites visible. Sticky header with the component
name.

PLATFORM
[Q4 — adapter taille / padding]

REQUIREMENTS
- TypeScript: typed props interface exported
- Accessibility: keyboard navigation, ARIA attributes, focus-visible
- Transitions: 150ms ease on hover/focus changes
- No external deps beyond React and Tailwind

BRAND CONTEXT
[Bloc étape 3. Fallback: "Neutral modern: slate-900 text, white surface,
indigo-600 primary, 8px radius, system font."]

DELIVERABLES
- src/components/[ComponentName].tsx — le composant
- src/App.tsx — showcase page rendant tout
- Lovable share link ready
```

## 5. Livrer

Format standard: prompt dans un bloc de code, lien Lovable, instructions.

## Anti-patterns

- Un composant = un prompt. Pour un système complet (button + input + card), produire un prompt par composant ou basculer vers prototype-web-screen pour un design system showcase.
- Toujours montrer variantes et états côte à côte (format gagnant du benchmark).
- Prompt en anglais, max 220 mots.
