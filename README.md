# tb-prototype

Plugin de prototypage rapide pour les PMPO Thirdbridge. Génère des prompts Lovable optimisés à partir d'un brief court, en intégrant le contexte du client (screenshots, Figma, URL déployée, ou repo).

Issu du benchmark interne de prototypage IA (juin 2026) qui a positionné Lovable en tête sur la fidélité visuelle, le partage de lien et le volume mensuel.

## Skills incluses

| Skill | Quand l'utiliser | Exemple de déclencheur |
|---|---|---|
| `prototype-mobile-screen` | Un seul écran mobile (login, profil, liste, formulaire) | « prototype mobile de connexion » |
| `prototype-web-screen` | Une seule page web ou desktop (dashboard, landing, settings) | « page web pour un dashboard de KPI » |
| `prototype-component` | Un composant isolé avec ses variantes et états | « prototype d'un bouton avec ses états » |
| `prototype-flow` | Parcours multi-écrans connectés (2 à 5 écrans) | « flow d'inscription en 3 étapes » |

## Comment ça fonctionne

Chaque skill suit le même workflow en 5 étapes:

1. **Confirmer l'intent** — reformulation en une phrase
2. **Collecter les specs** — 3 à 4 questions à choix (AskUserQuestion)
3. **Ingérer le contexte client** — screenshots, Figma, URL ou repo
4. **Assembler le prompt Lovable** — template optimisé, environ 200 mots
5. **Livrer au PMPO** — prompt prêt à coller + lien Lovable

Le PMPO copie le prompt dans Lovable.dev, attend la génération, et partage le lien public au client. Cycle complet visé: moins de 5 minutes.

## Sources de contexte client supportées

| Source | Mécanisme |
|---|---|
| Screenshots / images | Lecture directe via `Read` |
| Lien Figma | Figma MCP si connecté (`get_design_context`, `get_screenshot`) |
| URL d'un site déployé | `mcp__workspace__web_fetch`, fallback Chrome MCP |
| Repo Git local | `Glob` sur `tailwind.config.*`, `*.css`, `tokens.*`, `theme.*` |
| Aucun contexte | Fallback neutre moderne |

Aucune source n'est obligatoire: une skill sans contexte produit un prompt avec un fallback de palette neutre.

## Stack par défaut

React 18 + TypeScript + Tailwind CSS. Ajustable si le repo client impose une autre stack (Vue, Next, Svelte). Le PMPO peut le préciser dans sa demande.

## Format de sortie

Tous les prompts générés sont en anglais (Lovable performe mieux), tiennent en 200 à 320 mots selon la skill, et incluent:

- Stack et contraintes techniques
- Goal en 1-2 phrases
- Requirements (types d'écran, états, variantes)
- Brand context (extrait du client ou fallback)
- Deliverables et instructions de partage

## Installation

1. Récupérer le fichier `tb-prototype.plugin`
2. Dans Cowork, glisser-déposer le fichier ou utiliser le menu d'installation des plugins
3. Les 4 skills apparaissent dans la liste et se déclenchent automatiquement sur les phrases listées au-dessus

## Limites

- Le prototype Lovable est **un prototype**, pas du code de production: pas de tests, pas de gestion d'état complexe, pas d'intégration backend.
- Le PMPO doit avoir un compte Lovable actif pour utiliser le prompt généré.
- La qualité du résultat dépend de la qualité du contexte client fourni: plus le brief et les assets sont précis, meilleur sera le rendu.

## Évolutions prévues

- `prototype-from-figma` dédié, avec extraction profonde via Figma MCP
- `prototype-variants` pour comparer 2 directions visuelles d'un même écran
- Support de Bolt et Claude Code en sortie alternative
