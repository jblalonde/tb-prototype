---
name: prototype-flow
description: >
  Use this skill when the user wants to prototype a multi-screen user
  flow for a client: 2 to 4 connected screens with navigation between
  them. Trigger phrases include "parcours utilisateur", "user flow",
  "multi-écrans", "flow d'inscription", "onboarding", "checkout flow",
  "tunnel de conversion", "prototype de parcours". The skill collects
  flow requirements, ingests client context, and produces a Lovable
  prompt that generates all screens plus the navigation between them.
metadata:
  version: "0.1.0"
  author: "Thirdbridge"
---

# prototype-flow

## Quand cette skill se déclenche

L'utilisateur (PMPO Thirdbridge) veut prototyper **un parcours utilisateur multi-écrans**: onboarding, inscription, checkout, recherche puis détail puis confirmation, etc. Le prototype Lovable doit générer tous les écrans connectés avec la navigation fonctionnelle pour que le client puisse cliquer et tester le flow.

Si c'est un seul écran, utiliser `prototype-mobile-screen` ou `prototype-web-screen`. Si c'est un composant isolé, utiliser `prototype-component`.

## Comportement attendu

### Étape 1: Confirmer le flow

Reformuler en une phrase qui décrit le parcours complet. Exemple: « Tu veux un parcours d'inscription en 3 écrans: email, vérification OTP, profil. C'est ça? ».

### Étape 2: Collecter les spécifications

`AskUserQuestion` avec:

1. **Type de parcours** — pills: « Inscription / Onboarding », « Connexion / Récupération », « Checkout / Paiement », « Recherche + Détail + Action », « Création d'un item (wizard) », « Demande de support », « Autre (préciser) »
2. **Nombre d'écrans** — pills: « 2 écrans », « 3 écrans (recommandé) », « 4 écrans », « 5 écrans (max) »
3. **Plateforme** — pills: « Mobile », « Web / Desktop », « Responsive »
4. **Navigation** — pills: « Linéaire (next / back) », « Avec étapes visibles (stepper) », « Avec retour libre (sidebar / breadcrumb) »

Demander en suivi (texte libre): « Décris brièvement chaque écran (1 ligne par écran). »

### Étape 3: Ingérer le contexte client

Même mécanique que les autres skills (screenshots / Figma / URL / repo / aucun).

Extraction pour un flow:
- 4 à 6 couleurs avec usage (primary, success, error, neutral)
- Typographie (heading + body)
- Patterns de boutons (primary CTA visible sur chaque écran)
- Pattern de stepper ou navigation si visible

### Étape 4: Assembler le prompt Lovable

Template:

```
Build a multi-screen user flow prototype.

STACK
React 18 + TypeScript + Tailwind CSS + React Router v6. All screens in a
single Vite project. Use BrowserRouter for navigation between screens.

GOAL
Build a [Q1, ex. "user signup flow"] in [Q2, ex. "3 screens"] for a
[Q3, ex. "mobile"] experience. Each screen is fully functional with
working navigation between them.

SCREENS
[Liste numérotée à partir de la description libre de Q5:
1. /step-1 — [description écran 1]
2. /step-2 — [description écran 2]
3. /step-3 — [description écran 3]]

NAVIGATION PATTERN
[Q4. Adapter:
- Linear: each screen has "Next" CTA (primary) and "Back" link (text).
- Stepper: persistent stepper component at top showing current step out
  of total, completed steps marked with checkmark.
- Free: sidebar (web) or breadcrumb (mobile) showing all steps as links.]

PLATFORM
[Q3 — adapter container width, padding, touch targets si mobile]

REQUIREMENTS
- Routing: react-router-dom v6 with BrowserRouter
- State: lift form state to the parent App component, pass via props or
  Context. Mock submission on the final screen with a setTimeout success.
- Each screen has: heading, sub-text, content, primary CTA, secondary
  action if relevant
- Validation on every input that the user types into (inline error
  messages, no toast)
- Accessibility: keyboard navigation, focus management on route change
  (move focus to the new heading), ARIA-live for validation errors
- Loading state on the final submit (button shows spinner)

BRAND CONTEXT
[Bloc de l'étape 3. Fallback: "Neutral modern: slate-900 text, white bg,
indigo-600 primary, green-600 success, red-600 error, 8px radius, Inter
font."]

DELIVERABLES
- src/App.tsx — Router setup and shared state
- src/screens/Step1.tsx, Step2.tsx, etc. — one file per screen
- src/components/Stepper.tsx if Q4 = stepper
- Lovable share link ready to send to a stakeholder
```

### Étape 5: Livrer au PMPO

Format de livraison standard:

1. Prompt complet dans un bloc de code
2. Sous le bloc:
   - « Colle dans Lovable: https://lovable.dev/projects/new »
   - « Joins les images de référence si applicable »
   - « Le lien Lovable permet de naviguer entre les écrans comme dans un vrai prototype cliquable »

## Anti-patterns

- Limiter à 5 écrans max: au-delà Lovable génère du contenu superficiel sur chaque écran. Si le PMPO veut un parcours plus long, le découper en plusieurs prototypes.
- Ne pas écrire les microcopies (« Continue », « Retour »): Lovable les inventera, c'est OK pour un prototype.
- Toujours inclure la navigation fonctionnelle: un flow sans clics qui marchent n'a pas de valeur démo.
- Prompt final en anglais, max 320 mots (plus long que les autres skills à cause de la liste d'écrans).
