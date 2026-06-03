---
name: prototype-mobile-screen
description: >
  Use this skill when the user wants to prototype a single mobile screen
  for a client. Trigger phrases include "prototype mobile", "écran mobile",
  "écran iOS", "écran Android", "app mobile", "mockup mobile", "maquette
  mobile", "mobile screen prototype". The skill collects requirements,
  ingests client context (screenshots, Figma, URL, or repo), and produces
  a ready-to-paste Lovable prompt of about 200 words.
metadata:
  version: "0.1.0"
  author: "Thirdbridge"
---

# prototype-mobile-screen

## Quand cette skill se déclenche

L'utilisateur (un PMPO Thirdbridge) veut prototyper rapidement **un seul écran mobile** pour un client. Le but n'est pas de produire du code de production, c'est de générer en moins de 5 minutes un prompt Lovable de qualité que le PMPO va coller dans Lovable.dev pour obtenir un prototype partageable au client.

## Comportement attendu

Suivre les 5 étapes ci-dessous dans l'ordre. Ne sauter aucune étape. Toujours utiliser `AskUserQuestion` pour les questions à choix (pas de prose). Toujours valider avant de produire le prompt final.

### Étape 1: Confirmer l'intent en une phrase

Reformuler ce que tu comprends de la demande en une seule phrase courte. Exemple: « Tu veux un écran mobile de connexion avec email et option SSO. C'est ça? ». Si l'utilisateur a déjà tout dit dans son premier message, sauter cette étape.

### Étape 2: Collecter les spécifications via AskUserQuestion

Poser ces questions en un seul `AskUserQuestion` (jusqu'à 4 questions):

1. **Type d'écran** — pills: « Connexion / Inscription », « Profil utilisateur », « Liste / Feed », « Détail d'un item », « Formulaire », « Tableau de bord mobile »
2. **États à inclure** — multiSelect pills: « Loading », « Empty state », « Erreur », « Succès », « État par défaut seulement »
3. **Plateforme cible** — pills: « iOS-feel (style natif Apple) », « Android-feel (Material) », « Agnostique (web app mobile) »
4. **Ton visuel** — pills: « Corporatif sobre », « Moderne épuré », « Playful / coloré », « Suit la marque du client »

Si l'utilisateur tape une réponse libre, accepter telle quelle.

### Étape 3: Ingérer le contexte client

Demander via `AskUserQuestion` quelle source de contexte fournir, en multiSelect:

- « Screenshots ou images de référence » → demander à l'utilisateur d'uploader; lire chaque image avec le tool `Read` pour décrire ce que tu vois
- « Lien Figma » → demander l'URL figma.com; si le Figma MCP est connecté, utiliser `mcp__*__get_design_context` ou `mcp__*__get_screenshot` pour extraire couleurs, typo, composants
- « URL d'un site déployé du client » → utiliser `mcp__workspace__web_fetch` puis si insuffisant utiliser les outils Chrome MCP (`mcp__Claude_in_Chrome__navigate` + `mcp__Claude_in_Chrome__get_page_text`) pour extraire la charte
- « Repo Git local » → demander le chemin; utiliser `Glob` pour trouver `tailwind.config.*`, `*.css`, `tokens.*`, `theme.*`; lire les fichiers pour extraire couleurs, fonts, spacing
- « Aucun contexte, style neutre moderne » → sauter l'ingestion

Pour chaque source fournie, extraire au minimum:
- 3 à 6 couleurs clés (avec hex)
- Famille de typographie (sans-serif system par défaut si inconnue)
- 2 à 4 patterns de composants si visibles (boutons, cartes, inputs)

Stocker ces extractions dans un bloc de contexte qui sera injecté dans le prompt final.

### Étape 4: Assembler le prompt Lovable

Utiliser ce template exactement (remplir les `[CROCHETS]`):

```
Build a mobile screen prototype.

STACK
React 18 + TypeScript + Tailwind CSS. Single page component, no routing.
Mobile viewport target: 390x844 (iPhone 14). Use `max-w-[390px] mx-auto`
container with realistic device chrome (status bar, home indicator).

GOAL
[Reformulation 1-2 phrases de l'intent confirmé en étape 1]

REQUIREMENTS
- Screen type: [valeur de Q1]
- Platform feel: [valeur de Q3]
- Visual tone: [valeur de Q4]
- Include states: [liste de Q2, ex. "default, loading, error"]
  Render each state side-by-side or behind a state toggle (segmented control at the top of the canvas).
- Touch targets minimum 44x44pt
- Use semantic HTML and ARIA labels for screen readers
- Type scale: title 28-32px, body 16px, caption 13px

BRAND CONTEXT
[Bloc extrait de l'étape 3. Si rien fourni: "Use neutral modern palette: slate-900 text, white bg, single accent color #4F46E5. System sans-serif font."]

DELIVERABLES
- One React component exported as default
- All required states visible at once on the same canvas
- No external API calls, use realistic placeholder data
- Lovable share link ready to send to a non-technical stakeholder
```

### Étape 5: Livrer au PMPO

Sortie finale dans le chat:

1. Le prompt complet dans un bloc de code (3 backticks) pour copier-coller facile
2. Sous le bloc, ces 3 lignes:
   - « Colle ce prompt dans Lovable: https://lovable.dev/projects/new »
   - « Joins les images de référence si tu en as fourni »
   - « Le lien de partage Lovable est disponible dès que la génération est terminée (icône Share en haut à droite) »

Ne pas générer le code React soi-même. Ne pas appeler Lovable. La skill produit uniquement le prompt.

## Anti-patterns à éviter

- Ne pas demander la stack: c'est imposé (React + TS + Tailwind). Si le PMPO insiste pour une autre stack, l'accepter mais le noter explicitement dans le prompt.
- Ne pas inventer la charte: si aucun contexte client n'est fourni, utiliser le fallback neutre et ne pas extrapoler des couleurs de marque.
- Ne pas dépasser 250 mots dans le prompt final. Si le contexte client est volumineux, le compresser à 5-8 lignes max.
- Ne pas écrire le prompt en français: Lovable performe mieux en anglais. Les questions au PMPO restent en français.
