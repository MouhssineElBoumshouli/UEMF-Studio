# Consignes pour Claude Code (et tout agent IA)

Tu travailles sur UEMF Studio, un projet d'équipe de 3 personnes. Le dépôt GitHub est la source officielle.

## Règles obligatoires

1. **Ne jamais committer ni pousser sur `main`.** Travaille sur une branche `type/numéro-issue-description` (voir CONTRIBUTING.md).
2. **Ne travaille que sur une issue existante.** Si on te demande du travail sans issue, demande le numéro de l'issue ou propose de la créer.
3. **Tout passe par une Pull Request** vers `main`, avec `Closes #N` dans la description. Ne fusionne jamais une PR toi-même.
4. **Tu ne décides pas de l'architecture.** Avant d'ajouter une dépendance, de changer la structure des dossiers, la gestion d'état, le modèle de données ou un service externe : arrête-toi et demande. Si l'équipe valide, cela devient un ADR dans `docs/decisions/`.
5. **Suis les décisions déjà prises** dans `docs/decisions/`. Si une demande les contredit, signale-le au lieu de coder.
6. Termine chaque message de commit par une ligne `Co-Authored-By:` qui te nomme.
7. Ajoute le label `ai-assisted` aux PR que tu ouvres.

## État du projet

Phase de mise en place. **Ne crée pas de code Flutter** tant qu'un ADR n'a pas fixé la structure du projet.

## Où trouver quoi

- Règles d'équipe : `CONTRIBUTING.md`
- Décisions : `docs/decisions/`
- Règles IA : `docs/decisions/0002-usage-des-ia.md`
