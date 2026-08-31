---
id: ADR-002
title: Choix du framework frontend
status: Approved
date: 2026-08-30
related_stories: [STORY-001, STORY-002, STORY-003, STORY-004, STORY-005, STORY-006, STORY-007, STORY-008]
---

# ADR-002 — Choix du framework frontend

## Status
Approved

## Context

Le site combine des pages majoritairement statiques (contenu éditorial en Markdown/MDX) et des zones nécessitant de l'interactivité côté client avec état (connexion via OTP, saisie et consultation de routines personnelles liées à un compte utilisateur). Le choix initial d'Astro + Markdown/MDX, envisagé pour un site purement éditorial, doit être revalidé maintenant que le périmètre inclut des fonctionnalités authentifiées et dynamiques (cf. ADR-001).

## Decision

Le frontend est construit avec **Astro**, en conservant les pages de contenu (concepts, philosophes, citations) en rendu statique (Markdown/MDX, generation au build), et en utilisant des **îlots interactifs** (Astro Islands, avec une librairie de composants légère type Preact ou React selon préférence ultérieure du Developer) pour les zones nécessitant de l'état côté client : formulaire de connexion OTP, saisie de routine, affichage de l'historique personnel.

Ces îlots interactifs communiquent directement avec Supabase via son client JavaScript (auth + requêtes à la base avec Row Level Security), sans nécessiter de serveur applicatif intermédiaire pour le MVP.

## Alternatives considered

- **Passage complet à un framework full-stack (Next.js, SvelteKit, Remix)** : offrirait un modèle unifié SSR/CSR plus naturel pour gérer l'authentification et les données dynamiques nativement. Écarté pour le MVP — le contenu éditorial reste majoritaire en volume, et Astro permet de conserver les gains de performance et de simplicité du rendu statique pour cette majorité de pages, tout en couvrant le besoin dynamique via les islands. Un changement de framework introduirait un coût de réapprentissage et de réécriture non justifié à ce stade.
- **Astro en mode SSR complet (adapter serveur)** : possible mais non nécessaire — le besoin d'état dynamique est localisé à quelques zones précises (auth, routines), pas à l'ensemble du site. Le mode hybride (statique par défaut + islands ciblés) est plus proportionné et reste compatible avec le déploiement Cloudflare Pages retenu en ADR-001.
- **Rendu 100% côté client (SPA pure, ex. React seul sans Astro)** : écarté — sacrifierait les bénéfices SEO et de performance du rendu statique pour les pages de contenu, qui restent la majorité du site et un objectif explicite du projet.

## Rationale

- **Cohérence avec ADR-001** : Astro en mode hybride (statique + islands) fonctionne nativement avec Cloudflare Pages sans nécessiter d'adapter serveur complexe.
- **Proportionnalité** : la majorité du site (contenu éditorial) n'a pas besoin d'interactivité — conserver le rendu statique pour cette partie maximise performance et simplicité.
- **Confinement de la complexité dynamique** : limiter les islands aux zones réellement interactives (auth, routines) évite de propager de la complexité d'état à l'ensemble du site.
- **Valeur pédagogique** : le pattern "static-first avec islands ciblés" reste un apprentissage transférable et pertinent, sans réécriture complète de l'approche initialement envisagée.

## Consequences

- Le Developer devra choisir une librairie de composants pour les islands (Preact recommandé pour son poids réduit, à confirmer en story technique dédiée si nécessaire) — décision mineure ne nécessitant pas d'ADR séparé sauf si elle s'avère structurante à l'usage.
- Les tests e2e (QA) devront couvrir spécifiquement les zones dynamiques (auth, saisie de routines), qui constituent le risque technique principal du site comparé aux pages statiques.
- Toute évolution future vers un besoin de rendu dynamique plus étendu (ex: personnalisation poussée du contenu éditorial) devra faire l'objet d'un nouvel ADR réévaluant le passage à un mode SSR plus complet.
