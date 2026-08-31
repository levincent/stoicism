---
last_updated: 2026-08-31
status: living-document
owner: architect
---

# Standards de code

Ce document fixe les conventions techniques opposables pour tout code applicatif écrit dans `src/`. Il découle des contraintes posées par ADR-001 (hébergement/backend) et ADR-002 (framework frontend). Le Developer s'y conforme ; toute impossibilité pratique est remontée à l'Architect plutôt que contournée silencieusement (cf. `.claude/agents/developer.md`).

## Langage et typage

- TypeScript pour tout code applicatif (`.ts` / `.tsx`), en mode `strict` activé dans `tsconfig.json`. Pas de `any` implicite ni explicite sauf à la frontière d'une librairie tierce non typée, avec commentaire justifiant l'exception.
- JavaScript simple toléré uniquement pour de la config d'outillage (`*.config.js`) quand l'outil l'exige.

## Structure du projet (Astro)

- `src/pages/` — routes du site (pages statiques éditoriales + pages hébergeant les islands).
- `src/content/` — collections de contenu (concepts, philosophes, citations) en Markdown/MDX, avec schéma de frontmatter validé via `content/config.ts` (Zod, natif Astro). Le frontmatter doit respecter la terminologie de `docs/product/glossary.md`.
- `src/components/` — composants Astro statiques (`.astro`) et composants d'îlots interactifs (`.tsx`), séparés par sous-dossier (`components/static/`, `components/islands/`) pour rendre visible au premier coup d'œil ce qui embarque du JS client.
- `src/layouts/` — layouts partagés.
- `src/lib/` — code transverse non-UI (client Supabase, helpers de validation, types partagés).

## Îlots interactifs (Astro Islands)

Portée strictement limitée à ce que pose ADR-002 : connexion OTP, saisie de routine, historique. Aucune autre zone du site ne doit devenir un island par confort de développement — le rendu statique reste la valeur par défaut.

- Librairie de composants pour les islands : au choix du Developer (Preact recommandé pour son poids réduit, cf. ADR-002 Consequences) — une fois choisie pour le premier island, elle est réutilisée pour tous les suivants sans re-débat, sauf ADR ultérieur.
- Directive client la plus restrictive possible pour chaque island (`client:idle` ou `client:visible` par défaut, `client:load` seulement si l'interactivité doit être disponible immédiatement — ex. formulaire de connexion).
- Pas de librairie de gestion d'état globale (Redux, Zustand, etc.) : chaque island gère son propre état local. Si un besoin de partage d'état entre islands apparaît, c'est un signal à remonter à l'Architect avant d'introduire une dépendance.

## Accès aux données (Supabase)

- Un seul point d'instanciation du client Supabase (`src/lib/supabase.ts`), réutilisé partout — pas de nouvelle instance par composant.
- Clé publique (`anon key`) uniquement côté client. La clé `service_role` n'est **jamais** référencée dans du code exécuté côté client ni committée — variables d'environnement uniquement, hors du contrôle de version (`.env`, listé dans `.gitignore`, avec `.env.example` documentant les clés attendues sans valeurs réelles).
- L'isolation des données par utilisateur (AC4 de STORY-005) repose sur Row Level Security côté Postgres, pas sur une logique de filtrage côté client. Le code applicatif ne doit jamais simuler une vérification d'appartenance des données en JavaScript en lieu et place d'une policy RLS — RLS est la seule source de vérité pour l'isolation.
- Toute requête à une table contenant des données personnelles doit s'appuyer sur une policy RLS existante ; l'ajout d'une table sans policy RLS associée est un défaut bloquant, pas un oubli à corriger plus tard.

## Nommage

- Fichiers de composants : `PascalCase.astro` / `PascalCase.tsx`.
- Fichiers non-composants (helpers, config) : `kebab-case.ts`.
- Variables, fonctions : `camelCase`. Types et interfaces : `PascalCase`.
- Identifiants de code (variables, fonctions, types, noms de fichiers) en anglais, y compris pour un domaine métier francophone — cohérence avec l'écosystème JS/TS. Le contenu éditorial et les libellés utilisateur restent en français, conformes au glossaire.

## Style et qualité

- ESLint + Prettier, configuration unique au niveau racine du dépôt, appliquée sans dérogation locale par fichier sauf cas exceptionnel documenté en commentaire.
- Un composant / module = une responsabilité. Pas d'abstraction ou de couche de configuration ajoutée par anticipation d'un besoin futur non exprimé dans une story — cohérence avec le principe de proportionnalité posé en ADR-001/ADR-002.
- Pas de gestion d'erreur ni de validation pour des cas qui ne peuvent pas se produire compte tenu des garanties d'Astro/Supabase/TypeScript. La validation applicative se concentre sur les frontières réelles : saisie utilisateur (formulaires) et réponses d'API externes.

## Secrets et configuration

- Aucun secret (clé API, token) en dur dans le code ou committé, y compris dans des fichiers de test ou des exemples. Variables d'environnement exposées au build Astro via le préfixe `PUBLIC_` uniquement pour ce qui est réellement destiné au client.
- La stratégie de secrets management pour le CI/CD sera précisée par un futur DevSecOps Architect (cf. ADR-001, Consequences) ; en attendant, aucun secret n'est stocké ailleurs que dans la configuration d'environnement du fournisseur d'hébergement (Cloudflare Pages) ou un `.env` local non versionné.

## Git et revue

- Un commit ne mélange pas plusieurs stories sans raison explicite.
- Toute PR référence la story concernée (`STORY-XXX`) et, le cas échéant, l'ADR dont elle découle.
- Aucun merge direct sur `main` — la PR attend une approbation humaine explicite (règle non négociable du projet, cf. `CLAUDE.md`).

## Historique des changements de ce document

| Date | Changement |
|---|---|
| 2026-08-31 | Création initiale, découlant d'ADR-001 et ADR-002 |
