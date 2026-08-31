---
id: STORY-006
status: ready
owner: ba
related_adr: [ADR-001, ADR-002]
epic: C - Suivi des routines
---

# STORY-006 — Saisir une intention quotidienne

## User Story
En tant qu'utilisateur connecté, je veux saisir en texte libre ce sur quoi je veux me concentrer aujourd'hui (ex: patience, générosité, calme), afin de structurer ma pratique stoïcienne au quotidien.

## Acceptance Criteria
- [ ] AC1 : un utilisateur connecté peut saisir un texte libre court décrivant son intention pour la journée en cours.
- [ ] AC2 : une seule entrée existe par jour ; ressaisir le même jour modifie l'entrée existante plutôt que d'en créer une nouvelle.
- [ ] AC3 : la saisie est horodatée et associée uniquement à l'utilisateur connecté (isolation stricte, cf. STORY-005 AC4).

## Hypothèses formulées
- Pas de limite de caractères stricte imposée à ce stade au-delà d'une limite technique raisonnable (à définir par le Developer, ex: 280 caractères).
- Pas de catégorisation prédéfinie (tags, liste fermée) — texte totalement libre.

## Questions ouvertes pour le Business Owner
- Aucune à ce stade.
