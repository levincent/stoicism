---
id: STORY-003
status: ready
owner: ba
related_adr: [ADR-002]
epic: A - Contenu éditorial
---

# STORY-003 — Consulter des citations sourcées

## User Story
En tant que visiteur, je veux consulter des citations de philosophes stoïciens accompagnées de leur source précise, afin de m'assurer de leur authenticité.

## Acceptance Criteria
- [ ] AC1 : chaque citation affiche l'auteur, l'œuvre source, et une référence précise (livre/chapitre/section) lorsque disponible, conformément aux règles de `docs/product/glossary.md`.
- [ ] AC2 : une citation dont la source ne peut pas être vérifiée n'est pas publiée (garde-fou éditorial, validation par le Business Owner avant publication).
- [ ] AC3 : les citations sont accessibles à la fois depuis la page du philosophe associé et, si pertinent, depuis la page du concept associé.

## Hypothèses formulées
- La vérification de l'authenticité des citations reste manuelle et à la charge du Business Owner pour le MVP (cf. Étape 1 — pas de rôle dédié à la vérification factuelle à ce stade).

## Questions ouvertes pour le Business Owner
- Aucune à ce stade.
