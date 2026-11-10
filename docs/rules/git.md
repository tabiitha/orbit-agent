# Git

## Branches

- **Une branche = une issue.** Crée-la depuis la main branch à jour.
- **Nom** : `<type>/<numéro>-<description-courte>`.
  > Exemple : `fix/12-chevauchement-creneaux`
- **Pour la mettre à jour**, merge la main branch dedans. Ne la
  rebase que si elle n'a jamais été poussée (règle d'or 3).

## Commits

On suit [Conventional Commits](https://www.conventionalcommits.org/fr/v1.0.0/) :

```
<type>(<portée>): <ce que fait le changement>

[Optionnel : le pourquoi, si ce n'est pas évident.]

Refs: #<issue>
```

| Type | Pour |
| --- | --- |
| `feat` | Un nouveau comportement |
| `fix` | Une correction de bug |
| `refactor` | Une restructuration sans changement de comportement |
| `perf` | Une amélioration des performances |
| `test` | L'ajout ou la correction de tests |
| `docs` | De la documentation seule |
| `style` | De la mise en forme sans effet sur le code (espaces, formatage) |
| `build` | Le système de build ou les dépendances |
| `ci` | La configuration de l'intégration continue |
| `revert` | L'annulation d'un commit précédent |
| `chore` | Le reste : maintenance qui ne touche ni le code ni les tests |

- **La portée** est facultative : c'est la partie du code touchée, entre
  parenthèses (`api`, `auth`, `ui`…). Utilise les portées déjà présentes dans
  l'historique du projet.
- **La première ligne** vise 72 caractères, sans dépasser 100.
- **L'issue est référencée en pied** avec `Refs: #<numéro>`, pas dans la
  première ligne.
- **Des commits petits et cohérents** : un commit fait une seule chose.
- **Relis `git diff --staged` avant chaque commit** : rien d'étranger à la
  tâche, aucun secret (règle d'or 1).

> Exemples :
> ```
> fix(api): refuse les réservations qui se chevauchent
>
> Refs: #12
> ```
> ```
> feat(export): ajoute l'export des réservations en CSV
>
> Les clubs doivent transmettre ce fichier à leur fédération chaque mois.
>
> Refs: #15
> ```

## Pull requests

- **Une PR = une issue.** Pars du modèle `.github/pull_request_template.md`.
- **Lie l'issue** avec `Closes #<numéro>` dans la description : elle se fermera
  au merge.
- **Le titre suit le format de la première ligne des commits.**
- **Ouvre la PR quand la tâche est terminée** (section 4 d'`AGENTS.md`),
  pas avant.
- **Réponds aux retours de review par de nouveaux commits**, sans réécrire
  l'historique.
- **Le merge est fait par l'humain**, par défaut. Un agent ne merge une
  PR que si l'humain l'a demandé.
