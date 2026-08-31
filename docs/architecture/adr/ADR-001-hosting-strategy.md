---
id: ADR-001
title: Stratégie d'hébergement et de backend
status: Approved
date: 2026-08-30
related_stories: [STORY-005, STORY-006, STORY-007, STORY-008]
---

# ADR-001 — Stratégie d'hébergement et de backend

## Status
Approved

## Context

Le site combine deux natures de besoins distinctes :
- du contenu éditorial largement statique (concepts stoïciens, philosophes, citations sourcées) ;
- une fonctionnalité de suivi de routines personnelles nécessitant un compte utilisateur, une authentification par code OTP envoyé par email, et une persistance de données isolée par utilisateur et accessible depuis n'importe quel appareil.

Le trafic attendu est faible. Le projet est porté par un contributeur humain unique assisté d'agents IA, avec un objectif explicite de simplicité, de faible dette opérationnelle, et d'éviter le sur-engineering. Une infrastructure serveur permanente ne doit pas être choisie par défaut mais justifiée par un besoin réel.

## Decision

Le site sera hébergé sur **Cloudflare Pages**, avec **Supabase** comme backend-as-a-service pour :
- la base de données (Postgres géré, avec Row Level Security pour l'isolation des données par utilisateur) ;
- l'authentification (OTP par email, natif chez Supabase Auth, sans développement custom de flow d'authentification).

Le frontend reste construit avec Astro (confirmé séparément en ADR-002), avec des îlots interactifs pour les parties nécessitant de l'état côté client (connexion, saisie de routines).

## Alternatives considered

- **Astro + Vercel + Neon/Vercel Postgres + Auth.js** : viable, mais nécessite d'assembler soi-même le flow OTP via Auth.js plutôt que de bénéficier d'un mécanisme natif, pour un bénéfice marginal par rapport à Supabase.
- **AWS Amplify (Cognito + AppSync/DynamoDB + CloudFront)** : bonne valeur pédagogique AWS, mais complexité d'apprentissage et d'orchestration supérieure pour un gain fonctionnel équivalent au MVP. Écarté par choix explicite de privilégier la rapidité et la simplicité pour cette itération.
- **AWS "à la main" (Lambda + API Gateway + Cognito + RDS + SES)** : écarté — risque de sur-ingénierie disproportionné par rapport au besoin réel du MVP.
- **VPS OVH existant (Node + Postgres self-hosted)** : écarté — contredit le principe fondateur du projet de ne pas présupposer une infrastructure serveur permanente ; charge de maintenance (patchs, sécurité, backups) disproportionnée par rapport au trafic attendu.

## Rationale

- **Simplicité** : Supabase fournit l'authentification OTP et l'isolation des données (via Row Level Security) sans développement custom, ce qui réduit drastiquement la surface de code à écrire et à sécuriser pour le MVP.
- **Coût** : les tiers gratuits de Cloudflare Pages et Supabase couvrent largement le trafic attendu à ce stade.
- **Maintenance** : les deux plateformes sont managées ; ni patchs serveur, ni gestion de base de données à la charge du projet.
- **Sécurité** : Row Level Security sur Postgres est un mécanisme éprouvé pour garantir qu'un utilisateur ne peut accéder qu'à ses propres données, aligné avec l'exigence "strictement personnel et privé".
- **Risque de sur-architecture** : c'est l'option la plus proportionnée au besoin réel actuel parmi celles évaluées.
- **Valeur pédagogique AWS explicitement sacrifiée pour cette itération** : ce choix ne ferme pas la porte à une exploration AWS future si le projet évolue vers un besoin qui la justifie réellement (ex: montée en charge, besoin de services AWS spécifiques).

## Consequences

- Dépendance à deux fournisseurs tiers (Cloudflare, Supabase) plutôt qu'à une infrastructure auto-hébergée ou un unique fournisseur cloud généraliste.
- Migration future vers AWS possible mais non triviale si elle devient nécessaire (changement de paradigme BaaS → IaaS/PaaS).
- Le CI/CD Architect (à venir) devra intégrer le déploiement Cloudflare Pages et la gestion des migrations de schéma Supabase dans le pipeline.
- Le DevSecOps Architect (à venir) devra intégrer la gestion des secrets Supabase (clés API) dans la stratégie de secrets management.
