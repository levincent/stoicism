---
title: Vision produit — Stoïcisme
status: draft   # draft | validated (validation par le Business Owner requise)
owner: ba
last_updated: 2026-11-26
---

# Vision produit

> **Projet de vision à valider par le Business Owner avant tout développement.**
> Légende : ✅ = tiré des sources (README, `docs/architecture/architecture.md`, ADR, stories, glossaire) ; ⚠️ **HYPOTHÈSE** = non présent dans les sources, à confirmer ou corriger.

## 1. Objectif

✅ Proposer un site public autour du stoïcisme combinant :
- un **contenu éditorial** : concepts, philosophes, citations sourcées ;
- un **suivi personnel de pratique** : intentions quotidiennes et hebdomadaires en texte libre, pour les utilisateurs connectés, accessible depuis n'importe quel appareil.

✅ Objectif secondaire assumé : le projet sert de laboratoire pour une organisation de travail structurée entre agents IA spécialisés (BA, Architect, Developer, QA), avec gouvernance humaine à chaque étape structurante.

## 2. Public

✅ Deux profils, tirés des stories :
- **Visiteur** : consulte le contenu éditorial sans compte.
- **Utilisateur connecté** : visiteur disposant d'un compte (email + OTP), qui tient son suivi d'intentions.

⚠️ **HYPOTHÈSE H1** : le public cible est francophone (site et contenu en français, traductions françaises privilégiées d'après le glossaire). Aucune source ne fixe explicitement la langue ni le niveau de connaissance attendu du lecteur (débutant / initié).

⚠️ **HYPOTHÈSE H2** : trafic faible et cercle d'utilisateurs restreint au départ (l'ADR-001 mentionne un « trafic attendu faible » et la story de backlog un « cercle de test restreint »). Pas d'objectif d'audience chiffré.

## 3. Périmètre MVP

✅ Trois épics, couverts par les stories 001 à 008 :

| Épic | Contenu | Stories |
|---|---|---|
| A — Contenu éditorial | Pages et index de concepts et de philosophes, citations sourcées (garde-fou : pas de citation non vérifiée), navigation, responsive, SEO de base | 001, 002, 003, 004 |
| B — Compte utilisateur | Création de compte et connexion par OTP email, session persistante, déconnexion, isolation stricte des données | 005 |
| C — Suivi des routines | Intention quotidienne (une par jour), intention hebdomadaire (une par semaine), historique chronologique | 006, 007, 008 |

✅ Contraintes cadres : stack Astro / Cloudflare Pages / Supabase (ADR-001, ADR-002), terminologie conforme au glossaire, vérification des citations manuelle par le Business Owner.

✅ Prérequis technique (décision BO du 2026-11-26) : un socle Astro + CI minimale (build, lint, tests), prévu comme STORY-000, précède les stories fonctionnelles. L'ordre de livraison est : 000, 001, 002, 003, 004, puis 005 à 008, une PR par story. La STORY-007 peut attendre.

## 4. Hors périmètre

✅ Explicitement écartés du MVP :
- export de données en self-service (STORY-BACKLOG-001) — un processus manuel reste à prévoir en cas de demande RGPD (art. 20) dès que le site dépasse un cercle de test restreint ;
- notifications et rappels ;
- authentification par passkeys ;
- dimension sociale / partagée du suivi ;
- mot de passe, connexion sociale, 2FA supplémentaire ;
- catégorisation prédéfinie des intentions, recherche et filtres dans l'historique.

## 5. Critères de succès

Les sources ne définissent **aucun critère de succès chiffré**. Les propositions ci-dessous sont des **hypothèses** à valider ; seul le premier est directement dérivé des stories.

- ✅ **Livraison** : les stories 001 à 008 sont `done`, chacune avec ses Acceptance Criteria vérifiés par le QA, et mergées après démo et accord du BO.
- ⚠️ **HYPOTHÈSE H3 — contenu** : au lancement, les six philosophes du glossaire disposent d'une page, chacun avec au moins une citation sourcée vérifiée. (Nombre de concepts à couvrir : non défini.)
- ⚠️ **HYPOTHÈSE H4 — qualité** : zéro fuite de données entre utilisateurs (isolation RLS vérifiée par test) ; CI verte sur chaque PR.
- ⚠️ **HYPOTHÈSE H5 — usage** : au moins un utilisateur autre que le BO a créé un compte et saisi des intentions. Aucun seuil de rétention ou d'audience n'est proposé faute de source.

## 6. Hypothèses à valider (récapitulatif)

| # | Hypothèse | Impact si fausse |
|---|---|---|
| H1 | Public francophone, niveau de lecteur non précisé | Langue du contenu, ton éditorial |
| H2 | Trafic et audience faibles, pas d'objectif chiffré | Dimensionnement, indicateurs d'usage |
| H3 | Lancement avec 6 philosophes et au moins une citation sourcée chacun | Seuil de « contenu suffisant » |
| H4 | Critères de qualité : isolation des données et CI verte | Définition de « terminé » |
| H5 | Succès d'usage = au moins un utilisateur tiers actif | Mesure du succès du MVP |

## 7. Questions ouvertes pour le Business Owner

1. Quelle langue et quel public cible (débutants, curieux, lecteurs initiés) ? (H1)
2. Quels objectifs de succès retenez-vous, chiffrés ou non : volume de contenu, nombre d'utilisateurs, usage de la pratique ? (H3 à H5)
3. Combien de concepts doivent être publiés pour considérer le MVP éditorial comme complet ? (aucune liste de concepts dans les sources, contrairement aux philosophes)
4. Le site doit-il être mis en production publique avec comptes réels dès le MVP, ou rester en cercle de test ? Cela déclenche ou non la vigilance RGPD.
5. Le découpage de la semaine lundi-dimanche (STORY-007) reste à confirmer ; différé à la demande du BO.

## Historique

| Date | Changement |
|---|---|
| 2026-11-26 | Projet initial (BA), en attente de validation du Business Owner |
