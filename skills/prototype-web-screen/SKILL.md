---
name: prototype-web-screen
description: >
  Use this skill when the user wants to prototype a single web or desktop
  screen for a client. Trigger phrases include "page web", "écran web",
  "dashboard", "landing page", "site web", "écran desktop", "tableau de
  bord", "web screen prototype". The skill collects requirements, ingests
  client context (screenshots, Figma, URL, or repo), and produces a
  ready-to-paste Lovable prompt of about 200 words.
metadata:
  version: "0.1.0"
  author: "Thirdbridge"
---

# prototype-web-screen

## Quand cette skill se déclenche

L'utilisateur (PMPO Thirdbridge) veut prototyper **une page web ou desktop** pour un client: dashboard, landing, paramètres, page de détail, liste, formulaire long. Sortie attendue: un prompt Lovable de qualité, prêt à coller dans Lovable.dev.

Si l'utilisateur parle d'écran mobile, utiliser `prototype-mobile-screen` à la place. Si c'est un seul composant isolé, utiliser `prototype-component`. Si c'est plusieurs écrans connectés, utiliser `prototype-flow`.

## Comportement attendu

### Étape 1: Confirmer l'intent

Reformuler la demande en une phrase courte. Sauter si déjà clair.

### Étape 2: Collecter les spécifications via AskUserQuestion

Poser ces questions en un seul `AskUserQuestion`:

1. **Type de page** — pills: « Tableau de bord (KPIs, charts) », « Landing page marketing », « Liste / Index », « Page de détail », « Formulaire (settings, profil) », « Empty state / Onboarding »
2. **Densité d'information** — pills: « Légère (focus, peu d'info) », « Moyenne (équilibrée) », « Dense (data-heavy, tables) »
3. **États à inclure** — multiSelect: « Loading », « Empty », « Erreur », « Succès », « État par défaut seulement »
4. **Ton visuel** — pills: « Corporatif sobre », « Moderne épuré (SaaS B2B) », « Éditorial (storytelling) », « Suit la marque du client »

### Étape 3: Ingérer le contexte client

Demander via `AskUserQuestion` la source de contexte (multiSelect):

- « Screenshots / images de référence » → lire avec `Read`
- « Lien Figma » → utiliser Figma MCP si connecté (`get_design_context`, `get_screenshot`)
- « URL d'un site déployé » → `mcp__workspace__web_fetch`, fallback Chrome MCP si JS-rendered
- « Repo Git local » → `Glob` sur `tailwind.config.*`, `*.css`, `tokens.*`, `theme.*` puis `Read`
- « Aucun contexte, style neutre SaaS moderne »

Extraire pour chaque source:
- 4 à 8 couleurs (avec hex), distinguer primary / accent / neutral
- Famille de typographie (heading + body si différents)
- 3 à 5 patterns de composants visibles (header, sidebar, cards, table, buttons)
- Style des bordures, ombres, border-radius si identifiable

### Étape 4: Assembler le prompt Lovable

Template à remplir:

```
Build a web screen prototype.

STACK
React 18 + TypeScript + Tailwind CSS. Single page component. Responsive
from 1024px to 1920px. No backend, all data is realistic mock data.

GOAL
[Reformulation 1-2 phrases de l'intent]

REQUIREMENTS
- Page type: [Q1]
- Information density: [Q2]
- Include states: [Q3] — render each state in a separate section with a
  clear label, all visible on the same scroll.
- Visual tone: [Q4]
- Responsive: container max-w-7xl mx-auto, sensible breakpoints
- Accessibility: semantic HTML, keyboard navigation, focus states, ARIA
- Type scale: h1 36px, h2 28px, h3 20px, body 15px, caption 13px

LAYOUT GUIDELINES
[Si Q1 = dashboard: "Use a 12-column grid. Top bar with title and primary
action. KPI row of 3-4 cards. Main chart area below. Secondary table at
the bottom."
Si Q1 = landing: "Hero with headline + CTA + visual. Three feature blocks.
Social proof row. Final CTA section."
Si Q1 = liste: "Top bar with search, filters, primary action. Data table
or card grid below. Pagination at the bottom."
... adapter selon Q1]

BRAND CONTEXT
[Bloc de l'étape 3. Fallback si rien: "Neutral modern SaaS palette: slate
neutrals, indigo-600 primary, white surface, subtle shadows, 8px radius.
Inter font."]

DELIVERABLES
- One React component exported as default
- All states visible at once
- Realistic placeholder data (no Lorem Ipsum)
- Lovable share link ready
```

### Étape 5: Livrer au PMPO

Même format que `prototype-mobile-screen`:

1. Prompt complet dans un bloc de code pour copier
2. Lien Lovable et instructions de partage

## Anti-patterns

- Pas de stack alternative imposée; rester sur React + TS + Tailwind sauf demande explicite du PMPO.
- Pas d'invention de copy: les CTAs et textes restent génériques (« Action principale », « En savoir plus »).
- Prompt final en anglais, max 280 mots.
- Si le PMPO veut plusieurs pages connectées, basculer vers `prototype-flow` au lieu de cette skill.
