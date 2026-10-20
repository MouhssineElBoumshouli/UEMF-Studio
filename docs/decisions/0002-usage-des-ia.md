# ADR-0002 : Règles d'usage des IA

- **Statut :** Proposée
- **Date :** 2026-10-20
- **Issue :** #1
- **Décidé par :** l'équipe (règles posées par @MouhssineElBoumshouli)
- **Aide d'une IA :** oui — Claude a rédigé ce texte à partir des règles données par l'équipe

## Le problème

On utilise plusieurs IA (Claude, Claude Code, Codex). Elles écrivent vite, mais elles peuvent prendre des décisions sans qu'on s'en rende compte : une librairie, une structure de dossiers, un modèle de données. Si chaque IA décide de son côté, le projet devient incohérent.

## La décision

1. **Claude (chat)** sert à réfléchir : comparer des options, préparer un ADR.
2. **Claude Code et Codex** servent à implémenter une issue déjà décidée.
3. Les IA **ne décident pas de l'architecture**. Elles proposent, l'équipe décide.
4. Toute décision importante prise avec l'aide d'une IA est **écrite dans `docs/decisions/`** et passe par une PR.
5. Les IA suivent les mêmes règles que nous : issue, branche, PR. Jamais de commit sur `main`.
6. La personne assignée à l'issue **relit et assume** le code écrit par l'IA.
7. On marque le travail aidé par une IA : label `ai-assisted` et ligne `Co-Authored-By` dans le commit.
8. Les consignes pour les agents sont dans `CLAUDE.md` et `AGENTS.md`. Ces fichiers suivent ce document.

## Ce que ça change

- Avant de demander du code à un agent, il faut une issue claire.
- Si un agent veut ajouter une dépendance ou changer la structure, il s'arrête et demande.
- On garde une trace écrite de pourquoi le projet est comme il est.
