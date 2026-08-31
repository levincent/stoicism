---
last_updated: 2026-08-30
status: living-document
---

# Architecture — Vue d'ensemble

## Résumé

Le site est un site hybride : contenu éditorial statique (Markdown/MDX via Astro) combiné à des fonctionnalités personnelles authentifiées (suivi de routines) reposant sur Supabase (base de données Postgres + authentification OTP par email).

## Stack retenue

| Couche | Choix | ADR associé |
|---|---|---|
| Frontend | Astro (statique + islands interactifs) | ADR-002 |
| Hébergement | Cloudflare Pages | ADR-001 |
| Backend / BaaS | Supabase (Postgres + Auth) | ADR-001 |
| Authentification | OTP par email (Supabase Auth) | ADR-001 |
| Isolation des données | Row Level Security (Postgres) | ADR-001 |

## Schéma de principe

```
┌─────────────────────────────────────────┐
│              Cloudflare Pages             │
│  ┌─────────────────┐  ┌────────────────┐ │
│  │  Pages statiques │  │  Islands (JS)  │ │
│  │  (concepts,      │  │  - Connexion   │ │
│  │  philosophes,    │  │  - Saisie      │ │
│  │  citations)      │  │    routine     │ │
│  │                  │  │  - Historique  │ │
│  └─────────────────┘  └───────┬────────┘ │
└────────────────────────────────┼──────────┘
                                  │ (client JS Supabase)
                                  ▼
                     ┌─────────────────────────┐
                     │        Supabase          │
                     │  - Auth (OTP email)      │
                     │  - Postgres + RLS        │
                     └─────────────────────────┘
```

## Périmètre fonctionnel couvert (MVP)

- Contenu éditorial : concepts, philosophes, citations sourcées (Epic A).
- Compte utilisateur : création de compte et connexion par OTP email (Epic B).
- Suivi de routines : saisie d'intentions quotidiennes/hebdomadaires en texte libre, consultation de l'historique (Epic C).

## Hors périmètre MVP (backlog)

- Export de données utilisateur en self-service (note de vigilance RGPD : processus manuel à prévoir en cas de demande, cf. STORY backlog dédiée).
- Notifications/rappels.
- Authentification par passkeys (évolution possible si le socle applicatif se stabilise).
- Dimension sociale/partagée du suivi de routines (explicitement écartée pour l'instant).

## Historique des décisions

| ADR | Titre | Statut |
|---|---|---|
| ADR-001 | Stratégie d'hébergement et de backend | Approved |
| ADR-002 | Choix du framework frontend | Approved |

## Historique des changements de ce document

| Date | Changement |
|---|---|
| 2026-08-30 | Création initiale, suite à ADR-001 et ADR-002 |
