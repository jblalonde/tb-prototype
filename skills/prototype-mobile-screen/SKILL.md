---
description: Prototype a single mobile screen for a client. Collects requirements, ingests client context (screenshots, Figma, URL, or repo), and outputs a ready-to-paste Lovable prompt (~200 words).
when_to_use: >
  Use when the user wants to mock up or prototype a single mobile screen
  (login, profile, list, form, dashboard). Trigger phrases: "prototype
  mobile", "écran mobile", "écran iOS", "écran Android", "app mobile",
  "mockup mobile", "maquette mobile", "mobile screen prototype". For a
  web/desktop screen use prototype-web-screen; for an isolated component
  use prototype-component; for a multi-screen flow use prototype-flow.
---

But: générer en moins de 5 minutes un prompt Lovable de qualité que le PMPO collera dans Lovable.dev pour produire un prototype partageable au client.

Suivre les 5 étapes dans l'ordre. Toujours utiliser `AskUserQuestion` pour les choix (pas de prose). Toujours valider avant de produire le prompt final.

## 1. Confirmer l'intent

Reformuler en une phrase. Ex: « Tu veux un écran mobile de connexion avec email et option SSO. C'est ça? ». Sauter si déjà clair.

## 2. Collecter les specs

Un seul `AskUserQuestion`:

1. **Type d'écran** — pills: « Connexion / Inscription », « Profil utilisateur », « Liste / Feed », « Détail d'un item », « Formulaire », « Tableau de bord mobile »
2. **États à inclure** — multiSelect: « Loading », « Empty state », « Erreur », « Succès », « État par défaut seulement »
3. **Plateforme cible** — pills: « iOS-feel », « Android-feel (Material) », « Agnostique (web app mobile) »
4. **Ton visuel** — pills: « Corporatif sobre », « Moderne épuré », « Playful / coloré », « Suit la marque du client »

Accepter les réponses libres.

## 3. Ingérer le contexte client

`AskUserQuestion` multiSelect:

- **Screenshots / images** → lire avec `Read`
- **Lien Figma** → URL figma.com; si Figma MCP connecté, `get_design_context` ou `get_screenshot`
- **URL déployée** → `WebFetch`; fallback Chrome MCP si JS-rendered
- **Repo Git local** → `Glob` sur `tailwind.config.*`, `*.css`, `tokens.*`, `theme.*`, puis `Read`
- **Aucun contexte** → sauter

Extraire pour chaque source:
- 3 à 6 couleurs clés (avec hex)
- Famille de typographie (sans-serif system par défaut)
- 2 à 4 patterns de composants (boutons, cartes, inputs)

## 4. Assembler le prompt Lovable

Remplir ce template (remplacer `[CROCHETS]`):

```
Build a mobile screen prototype.

STACK
React 18 + TypeScript + Tailwind CSS. Single page component, no routing.
Mobile viewport target: 390x844 (iPhone 14). Use `max-w-[390px] mx-auto`
container with realistic device chrome (status bar, home indicator).

GOAL
[Reformulation 1-2 phrases]

REQUIREMENTS
- Screen type: [Q1]
- Platform feel: [Q3]
- Visual tone: [Q4]
- Include states: [Q2] — render each state side-by-side or behind a state
  toggle (segmented control at the top of the canvas).
- Touch targets minimum 44x44pt
- Semantic HTML and ARIA labels
- Type scale: title 28-32px, body 16px, caption 13px

BRAND CONTEXT
[Bloc extrait étape 3. Fallback: "Use neutral modern palette: slate-900
text, white bg, single accent #4F46E5. System sans-serif font."]

DELIVERABLES
- One React component exported as default
- All required states visible at once on the same canvas
- No external API calls, realistic placeholder data
- Lovable share link ready to send to a non-technical stakeholder
```

## 5. Livrer

1. Le prompt complet dans un bloc de code (3 backticks)
2. Sous le bloc:
   - « Colle dans Lovable: https://lovable.dev/projects/new »
   - « Joins les images de référence si applicable »
   - « Lien de partage disponible dès la génération terminée (Share, haut droite) »

Ne pas générer le code React. Ne pas appeler Lovable. La skill produit uniquement le prompt.

## Anti-patterns

- Stack imposée (React + TS + Tailwind). Si le PMPO insiste pour autre chose, l'accepter et le noter dans le prompt.
- Ne pas inventer la charte: si aucun contexte fourni, utiliser le fallback.
- Prompt final max 250 mots. Compresser le contexte client à 5-8 lignes.
- Prompt en anglais (Lovable performe mieux). Questions au PMPO en français.
