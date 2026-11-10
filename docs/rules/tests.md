# Tests

## Quoi tester

- **Le comportement, pas l'implémentation.** Teste ce que le code fait vu de
  l'extérieur (entrées, sorties, effets), pas la façon dont il le fait. Un
  refactor ne devrait pas casser un bon test.
- **Chaque critère d'acceptation de l'issue** est couvert par au moins un test.
- **Les cas limites** : valeurs vides, bornes, erreurs attendues, entrées
  invalides.
- **Pour un bug, d'abord le test.** Écris un test qui reproduit le bug et
  vérifie qu'il échoue. Corrige ensuite : le test passe et empêche le bug de
  revenir.

## Comment écrire un test

- **Suis l'existant** : même framework, même emplacement, mêmes conventions
  que les tests déjà en place.
- **Un test = un comportement**, avec un nom qui le décrit.
  > Exemple : `refuse une réservation qui chevauche un créneau existant`
- **Indépendant** : un test ne dépend ni de l'ordre d'exécution, ni d'un autre
  test.
- **Reproductible** : même résultat à chaque lancement. Contrôle ce qui varie
  (date, hasard, réseau) au lieu d'en dépendre.
- **Isolé du monde réel** : aucun service externe réel, aucune donnée de
  production. Remplace-les par des doublures à la frontière du système.
- **Rapide** : un test lent finit par ne plus être lancé.

## Prouver qu'un test sert à quelque chose

Un test qui passe toujours ne prouve rien. Pour vérifier le tien, annule
temporairement ton changement et relance-le : il doit échouer. S'il passe
encore, il ne teste pas ce que tu crois.

## Quand un test échoue

- **Corrige le code, pas le test.** Ne change pas une valeur attendue pour
  qu'elle corresponde à un résultat faux.
- **Si c'est le test qui te semble faux**, explique pourquoi et demande avant
  de le modifier.
- **Si un test échoue au hasard**, ne le relance pas jusqu'à ce qu'il passe.
  Signale-le : c'est un bug à part entière, qui mérite sa propre issue.

## Quand lancer les tests

- **Pendant le travail** : les tests liés à ce que tu modifies, souvent.
- **Avant de livrer** : la commande `check` de la section 2
  d'`AGENTS.md`, en entier.
