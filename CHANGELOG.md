# Changelog

Les versions suivent [Semantic Versioning](https://semver.org/lang/fr/).
Chaque version dit ce qu'il faut faire dans un projet déjà installé.

Pour publier une version : mettre à jour `.orbit-version` et ce fichier dans
la PR, puis créer le tag `vX.Y.Z` après le merge.

## 0.1.0 — 2026-11-12

Première version numérotée.

- Le modèle d'issue pose `needs-validation`, et la CI refuse une PR liée à
  une issue introuvable, fermée, ou qui porte `needs-validation` ou `blocked`.
- Les vérifications de la PR (issue, titre, commits) passent dans
  `orbit-policy.yml` (check `policy`), sur `pull_request_target`. `orbit.yml`
  ne garde que gitleaks et `check` (check `code`).
- La règle d'or 9 couvre `.github/`. Une PR qui touche `AGENTS.md`,
  `docs/rules/` ou `.github/` exige le label `rules-change`.
- Les PR de Dependabot et Renovate sont dispensées de `Closes #N`.
- `actions/checkout` est épinglé par SHA, et gitleaks est appelé sans son
  action, donc sans licence.
- Ajout de `.orbit-version`.

**Dans un projet déjà installé** : créer le label `rules-change`, et
remplacer le check requis `checks` par `policy` et `code`.
