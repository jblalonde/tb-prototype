---
description: Prototype a single web or desktop screen for a client (dashboard, landing, settings, list, form). Collects requirements, ingests client context, and outputs a ready-to-paste Lovable prompt (~250 words).
when_to_use: >
  Use when the user wants a single web/desktop page mock-up. Trigger
  phrases: "page web", "écran web", "dashboard", "landing page", "site
  web", "écran desktop", "tableau de bord", "web screen prototype". For
  mobile use prototype-mobile-screen; for an isolated component use
  prototype-component; for a multi-screen flow use prototype-flow.
---

Sortie attendue: un prompt Lovable de qualité, prêt à coller dans Lovable.dev.

## 1. Confirmer l'intent

Reformuler en une phrase. Sauter si déjà clair.

## 2. Collecter les specs

Un seul `AskUserQuestion`:

1. **Type de page** — pills: « Tableau de bord (KPIs, charts) », « Landing marketing », « Liste / Index », « Page de détail », « Formulaire (settings, profil) », « Empty state / Onboarding »
2. **Densité d'information** — pills: « Légère », « Moyenne », « Dense (data-heavy) »
3. **États à inclure** — multiSelect: « Loading », « Empty », « Erreur », « Succès », « État par défaut seulement »
4. **Ton visuel** — pills: « Corporatif sobre », « Moderne épuré (SaaS B2B) », « Éditorial », « Suit la marque du client »

## 3. Ingérer le contexte client

`AskUserQuestion` multiSelect:

- **Screenshots / images** → `Read`
- **Lien Figma** → Figma MCP si connecté (`get_design_context`, `get_screenshot`)
- **URL déployée** → `WebFetch`; fallback Chrome MCP si JS-rendered
- **Repo Git local** → `Glob` sur `tailwind.config.*`, `*.css`, `tokens.*`, `theme.*`, puis `Read`
- **Aucun contexte** → sauter

Extraire:
- 4 à 8 couleurs (hex), distinguer primary / accent / neutral
- Typographie (heading + body si différents)
- 3 à 5 patterns de composants (header, sidebar, cards, table, buttons)
- Bordures, ombres, border-radius si identifiables

## 4. Assembler le prompt Lovable

```
Build a web screen prototype.

STACK
React 18 + TypeScript + Tailwind CSS. Single page component. Responsive
from 1024px to 1920px. No backend, all data is realistic mock data.

GOAL
[Reformulation 1-2 phrases]

REQUIREMENTS
- Page type: [Q1]
- Information density: [Q2]
- Include states: [Q3] — each state in a separate section with a clear
  label, all visible on the same scroll.
- Visual tone: [Q4]
- Responsive: container max-w-7xl mx-auto, sensible breakpoints
- Accessibility: semantic HTML, keyboard navigation, focus states, ARIA
- Type scale: h1 36px, h2 28px, h3 20px, body 15px, caption 13px

LAYOUT GUIDELINES
[Adapter selon Q1:
- dashboard: 12-col grid, top bar with title + primary action, KPI row of
  3-4 cards, main chart area, secondary table at the bottom.
- landing: hero with headline + CTA + visual, three feature blocks,
  social proof row, final CTA section.
- liste: top bar with search + filters + primary action, data table or
  card grid, pagination at the bottom.
- autre: adapter au mieux]

BRAND CONTEXT
[Bloc étape 3. Fallback: "Neutral modern SaaS palette: slate neutrals,
indigo-600 primary, white surface, subtle shadows, 8px radius. Inter font."]

DELIVERABLES
- One React component exported as default
- All states visible at once
- Realistic placeholder data (no Lorem Ipsum)
- Lovable share link ready
```

## 5. Livrer

Comme prototype-mobile-screen: prompt dans un bloc, lien Lovable, instructions de partage.

## Anti-patterns

- Stack: React + TS + Tailwind sauf demande explicite.
- Pas d'invention de copy: CTAs/textes génériques (« Action principale », « En savoir plus »).
- Prompt en anglais, max 280 mots.
- Plusieurs pages connectées → basculer sur prototype-flow.
