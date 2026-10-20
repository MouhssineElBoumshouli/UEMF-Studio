# ADR-0001 : Règles de travail Git et GitHub

- **Statut :** Proposée
- **Date :** 2026-10-20
- **Issue :** #1
- **Décidé par :** l'équipe (règles posées par @MouhssineElBoumshouli)
- **Aide d'une IA :** oui — Claude a rédigé ce texte à partir des règles données par l'équipe

## Le problème

On est trois sur le même code. Sans règles, on écrase le travail des autres, on ne sait plus qui fait quoi, et les décisions se perdent dans des conversations.

## La décision

1. Le dépôt GitHub `MouhssineElBoumshouli/UEMF-Studio` est la **source officielle** du projet.
2. **Personne ne code directement sur `main`.**
3. Chaque tâche a **une issue** et **une seule personne** assignée.
4. Chaque fonctionnalité a **sa propre branche** (voir CONTRIBUTING.md pour les noms).
5. Tout changement entre dans `main` **par une Pull Request**, approuvée par au moins une autre personne.
6. Fusion en « Squash and merge ». La branche est supprimée après la fusion.

## Exception unique

Le dépôt était vide. Un premier commit avec un README minimal a été poussé sur `main` le 2026-10-20, seulement pour pouvoir ouvrir des Pull Requests. C'est la seule exception.

## Ce que ça change

- Il faut créer une issue même pour une petite tâche.
- L'historique de `main` reste propre : un commit = une tâche terminée.
- Il faut activer la protection de la branche `main` sur GitHub pour que la règle 2 soit forcée, pas seulement écrite.
