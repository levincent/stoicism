---
id: STORY-005
status: ready
owner: ba
related_adr: [ADR-001]
epic: B - Compte utilisateur
---

# STORY-005 — Créer un compte et se connecter

## User Story
En tant que visiteur, je veux créer un compte et me connecter via un code envoyé par email, afin d'accéder à mon suivi de routines personnel depuis n'importe quel appareil.

## Acceptance Criteria
- [ ] AC1 : un visiteur peut créer un compte en renseignant son adresse email.
- [ ] AC2 : la connexion se fait par code à usage unique (OTP) envoyé par email, saisi sur l'appareil où la connexion a été initiée.
- [ ] AC3 : une fois connecté, l'utilisateur reste authentifié de façon persistante (jusqu'à déconnexion explicite ou expiration raisonnable de session).
- [ ] AC4 : les données personnelles d'un utilisateur (routines) ne sont accessibles qu'à lui-même (isolation stricte, vérifiable via Row Level Security).
- [ ] AC5 : un utilisateur peut se déconnecter explicitement.

## Hypothèses formulées
- Pas de mot de passe, pas de connexion sociale (Google/Apple), pas de 2FA supplémentaire au MVP.
- Pas de vérification d'identité au-delà de la possession de l'adresse email.

## Questions ouvertes pour le Business Owner
- Aucune à ce stade.
