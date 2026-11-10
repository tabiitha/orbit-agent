# La boucle

Ce qui se passe à chaque retour en arrière du graphe (`docs/workflow/graph.md`) :
comment recommencer, et quand s'arrêter.

## La boucle de la tâche

*In progress ↺ : une vérification échoue.*

- **Lis l'erreur en entier** et explique-la avant de corriger. Ne modifie pas
  au hasard en espérant que ça passe.
- **Une correction à la fois**, puis revérifie.
- **Compte tes tentatives sur le même problème.** Une erreur différente ou
  plus de tests qui passent, c'est un progrès. La même erreur qui revient, non.
- **Après 3 tentatives sans progrès**, reviens au dernier état qui marchait,
  puis passe en **Blocked**.

## La boucle de la CI

*In review → In progress : la CI échoue.*

- **Lis les logs et reproduis l'échec en local** avant de corriger. Corrige sur
  la même branche, avec un nouveau commit.
- **Ne relance pas la CI en espérant qu'elle passe.** Un test instable mérite
  sa propre issue (voir `docs/rules/tests.md`).
- **Si l'échec ne vient pas de ton changement**, signale-le sans le corriger
  dans ta PR : c'est out of scope.

## La boucle de review

*In review → In progress : des changements sont demandés.*

- **Traite chaque remarque** : corrige, ou réponds avec ta raison si tu n'es
  pas d'accord. N'en ignore aucune en silence.
- **Réponds à chaque commentaire**, puis redemande une review.
- **Si la même remarque revient ou si le désaccord persiste**, l'humain
  tranche.

## La boucle de validation

*Needs validation ↺ : des changements sont demandés sur l'issue.*

- **Reformule l'issue selon les retours.** Si vous ne tombez pas d'accord après
  2 allers-retours, l'humain tranche.

## La boucle du projet

*Done → issue suivante.*

- **Seulement si l'humain t'a demandé d'enchaîner.** Par défaut, tu t'arrêtes
  après une issue.
- **L'ordre** : d'abord les dépendances, puis l'ordre des sub-issues de
  la parent issue.
- **Si une issue passe en Blocked**, continue avec une autre issue
  indépendante plutôt que d'attendre.
- **Arrête-toi** quand il n'y a plus d'issue Ready, ou quand la limite fixée
  par l'humain est atteinte (par exemple un nombre d'issues). Termine par un
  résumé de ce qui a été fait.

## Laisser une trace

Chaque fois que tu t'arrêtes en cours de route (Blocked, fin de session,
limite atteinte), laisse un commentaire dans l'issue. Il doit permettre à un
autre agent de reprendre sans tout redécouvrir, et rien de plus :

```
**Où j'en suis** : [ce qui est fait, en une phrase]
**Essayé** : [ce qui n'a pas marché, et pourquoi]
**Suite** : [la prochaine étape, ou la question en attente]
```

> Exemple :
> **Où j'en suis** : validation des chevauchements faite, les tests passent.
> **Essayé** : comparer les dates en heure locale, faux à cause des fuseaux.
> **Suite** : passer les comparaisons en UTC via `domain/time.ts`.

Une ligne par champ suffit le plus souvent. Supprime **Essayé** si rien n'a
échoué. Ne raconte pas ta démarche : les commits et la PR la montrent déjà.
