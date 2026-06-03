# tb-prototype

Plugin Claude Code pour générer des prompts Lovable à partir d'un brief court. Conçu pour les PMPO Thirdbridge.

## Skills

| Skill | Cas d'usage | Déclencheur |
|---|---|---|
| `prototype-mobile-screen` | Un écran mobile (login, profil, liste) | « prototype mobile de connexion » |
| `prototype-web-screen` | Une page web (dashboard, landing) | « page web pour un dashboard de KPI » |
| `prototype-component` | Un composant et ses variantes | « prototype d'un bouton avec ses états » |
| `prototype-flow` | Un parcours de 2 à 5 écrans connectés | « flow d'inscription en 3 étapes » |

## Installation

Glisser-déposer `tb-prototype.plugin` dans Cowork. Les skills s'activent automatiquement sur les phrases ci-dessus.

## Workflow

1. Le PMPO décrit son besoin → la skill reformule l'intent
2. 3-4 questions à choix pour cadrer les specs
3. Ingestion optionnelle du contexte client (voir ci-dessous)
4. Assemblage d'un prompt Lovable en anglais (~200 mots)
5. Le PMPO colle le prompt dans Lovable et partage le lien généré

Cycle visé: moins de 5 minutes.

## Sources de contexte

- **Screenshots** — lecture directe
- **Figma** — via Figma MCP si connecté
- **URL déployée** — fetch web + fallback Chrome MCP
- **Repo Git local** — extraction des tokens Tailwind/CSS
- **Aucune** — fallback neutre moderne

## Stack par défaut

React 18 + TypeScript + Tailwind. Ajustable si le repo client impose autre chose (Vue, Next, Svelte) — à préciser dans la demande.

## Limites

- C'est un prototype, pas du code de production: pas de tests, pas de backend, pas de gestion d'état complexe.
- Requiert un compte Lovable actif.
- La qualité du rendu dépend de la précision du brief et du contexte fourni.
