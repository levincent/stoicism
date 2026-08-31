# Instructions pour Claude Code sur ce projet

## Contexte

Ce projet combine un site éditorial statique (stoïcisme) et une fonctionnalité de suivi personnel de routines nécessitant compte utilisateur et backend. Le repository Git est la source de vérité du projet — ne pas présumer d'informations non présentes dans `docs/`.

## Avant toute tâche

1. Identifier le rôle pertinent pour la tâche demandée parmi `.claude/agents/` (`ba`, `architect`, `developer`, `qa`) et respecter strictement son périmètre (fichiers autorisés en lecture/modification, responsabilités, non-responsabilités).
2. Lire les ADR approuvés dans `docs/architecture/adr/` avant toute décision ou implémentation technique — ce sont des contraintes opposables, pas des suggestions.
3. Lire la story concernée dans `docs/product/stories/` et ses Acceptance Criteria avant d'implémenter quoi que ce soit.

## Règles non négociables

- Aucun agent ne merge sur `main`. Toute PR attend une approbation humaine explicite.
- Aucune décision d'architecture structurante n'est appliquée sans ADR au statut `Approved`.
- Un ADR approuvé n'est jamais modifié rétroactivement — il est remplacé par un nouvel ADR (`Superseded by ADR-YYY`) si nécessaire.
- Le Developer ne dévie jamais d'un ADR approuvé par préférence personnelle ; toute difficulté d'implémentation déclenche une demande de révision auprès de l'Architect, pas un contournement silencieux.
- Le QA travaille à partir des Acceptance Criteria de la story, pas à partir de ce que le Developer pense avoir implémenté.

## Structure du repository

Voir `README.md` pour la carte complète. En résumé :
- `docs/product/` — vision, stories, glossaire
- `docs/architecture/` — ADR et vue d'ensemble technique
- `docs/standards/` — conventions de code et de tests
- `src/` — code applicatif
- `tests/` — tests unitaires, intégration, e2e
- `.claude/agents/` — définitions des subagents

## En cas de doute

Expliciter les hypothèses formulées plutôt que de deviner silencieusement, et signaler les questions ouvertes nécessitant une validation humaine — conformément aux règles d'escalade définies dans chaque fichier d'agent.
