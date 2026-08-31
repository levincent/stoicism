---
last_updated: 2026-08-31
status: living-document
owner: architect
---

# Standards de tests

Ce document fixe le niveau de rigueur et les outils attendus pour les trois couches de test du projet, réparties entre deux rôles distincts :

- **Developer** — tests unitaires, dans `tests/unit/`.
- **QA** — tests d'intégration et end-to-end, dans `tests/integration/` et `tests/e2e/`.

Cette séparation (cf. `.claude/agents/developer.md` et `.claude/agents/qa.md`) est structurelle : le QA vérifie contre les Acceptance Criteria de la story, indépendamment de ce que le Developer pense avoir implémenté. Il n'écrit jamais de test unitaire, et le Developer n'écrit jamais de test d'intégration ou e2e.

## Outillage

- **Tests unitaires** : Vitest (intégration native avec l'environnement Vite/Astro).
- **Tests d'intégration** : Vitest également, mais exécutés contre une vraie instance Supabase (projet Supabase de test dédié ou instance locale via Supabase CLI) — jamais contre un mock du client Supabase pour ce qui touche à l'authentification ou aux policies RLS.
- **Tests e2e** : Playwright, piloté contre un build Astro servi localement ou un déploiement de preview Cloudflare Pages.

## Ce que chaque couche couvre

### Tests unitaires (Developer)

- Logique métier isolée : fonctions de `src/lib/`, validation de formulaire, transformation de données.
- Composants d'îlots interactifs testés en isolation (rendu, comportement au clic/saisie), sans réseau réel — le client Supabase est mocké ici, puisque l'objectif est de vérifier le comportement du composant, pas l'intégration réelle avec le backend.
- Ne couvrent pas : le rendu des pages statiques de contenu (pas de logique à tester), les policies RLS, les parcours multi-pages.

### Tests d'intégration (QA)

- Interactions réelles avec Supabase : une requête authentifiée en tant qu'utilisateur A ne doit jamais retourner ou modifier les données de l'utilisateur B (vérification directe des policies RLS, pas seulement du comportement de l'UI qui les appelle).
- Flow d'authentification OTP de bout en bout côté API (création de compte, réception/validation du code, persistance de session), sans nécessairement passer par le navigateur.
- Toute story dont l'AC mentionne explicitement une isolation ou une persistance de données (ex. STORY-005 AC3/AC4, STORY-006 AC2/AC3) doit avoir un test d'intégration dédié qui échouerait si la garantie était rompue — pas seulement un test qui vérifie le cas nominal.

### Tests e2e (QA)

- Parcours utilisateur complets dans un vrai navigateur, concentrés sur les zones dynamiques identifiées comme risque technique principal par ADR-002 (Consequences) : connexion, saisie de routine, consultation d'historique.
- Le contenu éditorial statique (concepts, philosophes, citations) ne nécessite pas de couverture e2e systématique — un test de navigation de base (la page se charge, la navigation fonctionne) suffit ; le contenu lui-même relève d'une vérification éditoriale par le Business Owner, pas d'un test automatisé.

## Traçabilité aux Acceptance Criteria

- Chaque test (unitaire, intégration ou e2e) référence dans sa description l'AC qu'il vérifie, au format `STORY-XXX AC# — description du cas`. Un test sans référence à un AC ou à un cas limite explicite n'est pas suffisant pour la Definition of Done.
- Le rapport QA (format défini dans `.claude/agents/qa.md`) associe à chaque AC un verdict individuel — pas de verdict global approximatif au niveau de la story.
- Pas de seuil de couverture de code imposé en pourcentage : l'exigence est que chaque AC soit couvert par au moins un test identifiable, conformément à la Definition of Done du Developer et du QA. Un pourcentage de couverture élevé sur du code non lié à un AC n'a pas de valeur ici et ne doit pas être recherché pour lui-même.

## Données de test

- Aucune donnée personnelle réelle dans les tests, y compris en local. Le projet Supabase de test est distinct du projet de production — jamais de test exécuté contre les données de production.
- Les fixtures de contenu éditorial utilisées en test (citations, noms de philosophes) doivent rester cohérentes avec `docs/product/glossary.md`, même dans un test unitaire, pour éviter qu'un test valide une terminologie que le contenu réel rejette.

## CI

- Le socle CI minimal (lint, build, tests) est posé par l'Architect (cf. `.claude/agents/architect.md`). Les trois couches de tests (unitaires, intégration, e2e) tournent en CI sur chaque PR ; un test d'intégration ou e2e en échec bloque le merge au même titre qu'un test unitaire — aucune couche n'est traitée comme secondaire.
- Un test identifié comme flaky (échec intermittent non reproductible) n'est pas silencieusement retiré de la CI ni mis en `skip` sans signalement explicite : Developer si le test est unitaire, QA sinon, avec remontée à l'Architect si la cause semble structurelle (ex. absence d'isolation entre tests d'intégration).

## Historique des changements de ce document

| Date | Changement |
|---|---|
| 2026-08-31 | Création initiale, découlant d'ADR-001 et ADR-002 |
