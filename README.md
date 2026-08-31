# Stoïcisme — Site éditorial et suivi de pratique personnelle

Site public autour du stoïcisme : contenu éditorial (concepts, philosophes, citations sourcées) et suivi personnel de routines quotidiennes/hebdomadaires pour les utilisateurs connectés.

Ce projet sert aussi de laboratoire pour expérimenter une organisation de travail structurée entre agents IA spécialisés (Business Analyst, Architect, Developer, QA), avec gouvernance humaine explicite à chaque étape structurante.

## Pour comprendre le projet rapidement

- **Vision produit** : `docs/product/vision.md` *(à rédiger)*
- **Stories et backlog** : `docs/product/stories/`
- **Décisions d'architecture (ADR)** : `docs/architecture/adr/`
- **Vue d'ensemble de l'architecture** : `docs/architecture/architecture.md`
- **Glossaire du domaine** : `docs/product/glossary.md`
- **Standards techniques** : `docs/standards/` *(à compléter)*
- **Définitions des agents Claude Code** : `.claude/agents/`

## Stack

- Frontend : Astro (statique + islands interactifs) — voir ADR-002
- Hébergement : Cloudflare Pages — voir ADR-001
- Backend / Auth : Supabase (Postgres + Row Level Security, authentification OTP par email) — voir ADR-001

## Gouvernance

Aucun agent ne merge seul sur `main`. Toute décision d'architecture structurante est soumise à l'approbation humaine avant application. Voir `.claude/agents/` pour le détail des rôles, responsabilités et règles d'escalade de chaque agent.

## Statut actuel

Système de travail établi (rôles, workflow, gouvernance, ADR initiaux, backlog MVP). Implémentation pas encore démarrée.
