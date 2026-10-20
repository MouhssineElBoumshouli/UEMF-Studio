# Comment on travaille sur UEMF Studio

Ce guide est court. Suis-le à chaque tâche.

## 1. Une tâche commence par une issue

- Avant de toucher au code, il faut une issue sur GitHub.
- Une issue = **une seule personne** assignée. Pas deux, pas zéro.
- Utilise un des modèles : Fonctionnalité, Bug ou Décision.
- Si la tâche est trop grosse pour une seule personne, coupe-la en plusieurs issues.

## 2. Une branche par tâche

On ne travaille **jamais** sur `main`.

Nom de la branche : `type/numéro-issue-description-courte`

| Type | Quand | Exemple |
|---|---|---|
| `feature/` | nouvelle fonctionnalité | `feature/12-ecran-connexion` |
| `fix/` | correction de bug | `fix/18-crash-au-demarrage` |
| `chore/` | outils, réglages, organisation | `chore/1-regles-du-projet` |
| `docs/` | documentation seulement | `docs/7-guide-installation` |

```bash
git checkout main
git pull
git checkout -b feature/12-ecran-connexion
```

## 3. Des commits clairs

- Un commit = un changement logique.
- Message court qui dit **ce que fait** le changement : `Ajoute le formulaire de connexion`.
- Si une IA a écrit une partie du code, ajoute à la fin du message :
  `Co-Authored-By: <nom de l'outil>` (par exemple Claude ou Codex).

## 4. Tout passe par une Pull Request

1. Pousse ta branche : `git push -u origin feature/12-ecran-connexion`
2. Ouvre une Pull Request vers `main`.
3. Remplis le modèle de PR. Écris `Closes #12` pour lier l'issue.
4. Demande une relecture à **au moins une autre personne** de l'équipe.
5. On ne fusionne pas sa propre PR sans approbation.
6. Fusion en **Squash and merge**. La branche est supprimée après.

## 5. Les IA dans le projet

Claude, Claude Code et Codex sont des aides. Ce sont les humains qui décident.

- **Claude** (chat) : pour réfléchir, comparer des options, préparer une décision.
- **Claude Code / Codex** : pour écrire du code **dans une issue déjà décidée**.
- Une IA ne choisit pas seule l'architecture, une librairie importante ou la structure des données.
- Si une IA propose une décision importante, on l'écrit dans `docs/decisions/` (voir plus bas) **avant** de coder.
- Mets le label `ai-assisted` sur l'issue ou la PR quand une IA a beaucoup aidé.
- La personne assignée est responsable du code, même si une IA l'a écrit. Relis-le avant d'ouvrir la PR.

Les règles pour les agents IA sont aussi dans [CLAUDE.md](CLAUDE.md) et [AGENTS.md](AGENTS.md).

## 6. Les décisions importantes (ADR)

Une décision est « importante » si elle est difficile à changer plus tard. Exemples :
- choix de l'architecture ou de la gestion d'état ;
- choix d'une base de données ou d'un service externe ;
- structure des dossiers ;
- règle d'équipe.

Comment faire :
1. Ouvre une issue avec le modèle **Décision**.
2. Copie [docs/decisions/0000-modele.md](docs/decisions/0000-modele.md) vers un nouveau fichier numéroté.
3. Ouvre une PR. L'équipe en parle dans la PR.
4. Une fois fusionnée, la décision est officielle.

Pour changer une décision, on écrit un nouvel ADR qui remplace l'ancien. On n'efface pas l'ancien.
