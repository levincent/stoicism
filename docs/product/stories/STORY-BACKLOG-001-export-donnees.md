---
id: STORY-BACKLOG-001
status: backlog
owner: ba
related_adr: []
epic: C - Suivi des routines
---

# STORY-BACKLOG-001 — Exporter mes données personnelles (hors MVP)

## User Story
En tant qu'utilisateur connecté, je veux pouvoir exporter mes données de routines, afin d'en conserver une copie ou de les récupérer si je le souhaite.

## Statut
Explicitement hors périmètre du MVP (décision Business Owner du 2026-08-30).

## Note de vigilance réglementaire
Le RGPD (article 20 — droit à la portabilité des données) reste applicable même en l'absence de fonctionnalité d'export en self-service. Un processus manuel minimal (extraction des données sur demande explicite d'un utilisateur, réalisée par le Business Owner via un accès direct à la base) doit rester possible tant que cette story n'est pas implémentée. Aucune action technique requise avant mise en production réelle avec des comptes utilisateurs actifs, mais ce point ne doit pas être oublié une fois le site utilisé au-delà d'un cercle de test restreint.

## Acceptance Criteria (à affiner lors de la mise en priorité)
- [ ] AC1 (brouillon) : un utilisateur connecté peut demander un export de ses données au format lisible (ex: JSON ou CSV).
