---
description: Prototype a multi-screen user flow (2 to 5 connected screens with working navigation). Collects flow requirements, ingests client context, and outputs a Lovable prompt that generates all screens plus navigation.
when_to_use: >
  Use when the user wants a clickable multi-screen prototype: onboarding,
  signup, checkout, search → detail → confirm, wizard creation. Trigger
  phrases: "parcours utilisateur", "user flow", "multi-écrans", "flow
  d'inscription", "onboarding", "checkout flow", "tunnel de conversion",
  "prototype de parcours". For a single screen use prototype-mobile-screen
  or prototype-web-screen; for an isolated component use prototype-
  component.
---

Le prototype Lovable doit générer tous les écrans connectés avec la navigation fonctionnelle pour que le client puisse cliquer et tester le flow.

## 1. Confirmer le flow

Reformuler en une phrase qui décrit le parcours complet. Ex: « Tu veux un parcours d'inscription en 3 écrans: email, vérification OTP, profil. C'est ça? ».

## 2. Collecter les specs

`AskUserQuestion`:

1. **Type de parcours** — pills: « Inscription / Onboarding », « Connexion / Récupération », « Checkout / Paiement », « Recherche + Détail + Action », « Création (wizard) », « Demande de support », « Autre (préciser) »
2. **Nombre d'écrans** — pills: « 2 écrans », « 3 écrans (recommandé) », « 4 écrans », « 5 écrans (max) »
3. **Plateforme** — pills: « Mobile », « Web / Desktop », « Responsive »
4. **Navigation** — pills: « Linéaire (next / back) », « Stepper », « Retour libre (sidebar / breadcrumb) »

Suivi (texte libre): « Décris brièvement chaque écran (1 ligne par écran). »

## 3. Ingérer le contexte client

Même mécanique (screenshots / Figma / URL / repo / aucun).

Extraction pour un flow:
- 4 à 6 couleurs avec usage (primary, success, error, neutral)
- Typographie (heading + body)
- Pattern de boutons (CTA primary visible sur chaque écran)
- Pattern de stepper / navigation si visible

## 4. Assembler le prompt Lovable

```
Build a multi-screen user flow prototype.

STACK
React 18 + TypeScript + Tailwind CSS + React Router v6. All screens in a
single Vite project. Use BrowserRouter for navigation between screens.

GOAL
Build a [Q1, ex. "user signup flow"] in [Q2, ex. "3 screens"] for a
[Q3, ex. "mobile"] experience. Each screen is fully functional with
working navigation.

SCREENS
[Liste numérotée à partir de la description libre:
1. /step-1 — [description]
2. /step-2 — [description]
3. /step-3 — [description]]

NAVIGATION PATTERN
[Q4. Adapter:
- Linear: each screen has "Next" CTA (primary) and "Back" link (text).
- Stepper: persistent stepper at top showing current step out of total,
  completed steps marked with checkmark.
- Free: sidebar (web) or breadcrumb (mobile) showing all steps as links.]

PLATFORM
[Q3 — adapter container width, padding, touch targets si mobile]

REQUIREMENTS
- Routing: react-router-dom v6 with BrowserRouter
- State: lift form state to parent App component, pass via props/Context.
  Mock submission on the final screen with a setTimeout success.
- Each screen: heading, sub-text, content, primary CTA, secondary action
- Validation on every input (inline error messages, no toast)
- Accessibility: keyboard nav, focus on heading after route change,
  ARIA-live for validation errors
- Loading state on final submit (button shows spinner)

BRAND CONTEXT
[Bloc étape 3. Fallback: "Neutral modern: slate-900 text, white bg,
indigo-600 primary, green-600 success, red-600 error, 8px radius, Inter
font."]

DELIVERABLES
- src/App.tsx — Router setup et shared state
- src/screens/Step1.tsx, Step2.tsx, etc. — un fichier par écran
- src/components/Stepper.tsx si Q4 = stepper
- Lovable share link ready to send to a stakeholder
```

## 5. Livrer

1. Prompt complet dans un bloc de code
2. Sous le bloc:
   - « Colle dans Lovable: https://lovable.dev/projects/new »
   - « Joins les images de référence si applicable »
   - « Le lien Lovable permet de naviguer entre les écrans comme dans un vrai prototype cliquable »

## Anti-patterns

- 5 écrans max: au-delà Lovable génère du contenu superficiel. Plus long → découper en plusieurs prototypes.
- Ne pas écrire les microcopies (« Continue », « Retour »): Lovable les inventera.
- Toujours inclure la navigation fonctionnelle: un flow sans clics qui marchent n'a pas de valeur démo.
- Prompt en anglais, max 320 mots (plus long à cause de la liste d'écrans).
