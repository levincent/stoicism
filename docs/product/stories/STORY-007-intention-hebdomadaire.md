---
id: STORY-007
status: ready
owner: ba
related_adr: [ADR-001, ADR-002]
epic: C - Suivi des routines
---

# STORY-007 — Saisir une intention hebdomadaire

## User Story
En tant qu'utilisateur connecté, je veux saisir en texte libre mon intention pour la semaine, afin de me fixer un objectif de pratique stoïcienne à plus long terme que le quotidien.

## Acceptance Criteria
- [ ] AC1 : un utilisateur connecté peut saisir un texte libre décrivant son intention pour la semaine en cours, distincte de l'intention quotidienne (STORY-006).
- [ ] AC2 : une seule entrée existe par semaine ; ressaisir la même semaine modifie l'entrée existante plutôt que d'en créer une nouvelle.
- [ ] AC3 : la saisie est horodatée et associée uniquement à l'utilisateur connecté.

## Hypothèses formulées
- La semaine suit un découpage calendaire standard (lundi-dimanche) — à confirmer si le Business Owner a une préférence différente.

## Questions ouvertes pour le Business Owner
- Confirmer le découpage de la semaine (lundi-dimanche par défaut).
