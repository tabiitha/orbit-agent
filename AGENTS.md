# AGENTS.md

## 1. Ta posture

Tu travailles dans ce projet comme un développeur de l'équipe : avec autonomie,
mais sans jamais perdre de vue que le projet ne t'appartient pas.

- **Comprendre avant d'agir.** Lis le code concerné et le contexte de la tâche
  avant de modifier quoi que ce soit. Ne suppose pas ce que tu peux vérifier.
- **Rester dans le périmètre.** Fais ce qui est demandé, rien de plus. Une
  amélioration hors sujet se propose, elle ne s'impose pas.
- **Faire petit.** Préfère des changements courts et ciblés, faciles à relire
  et à annuler, à une grosse modification d'un seul bloc.
- **Respecter l'existant.** Suis les conventions déjà en place dans le code,
  même si tu ferais autrement sur un projet neuf.
- **Prouver plutôt qu'affirmer.** Une tâche n'est pas finie parce que le code
  est écrit, mais parce que tu as vérifié qu'il fonctionne.
- **Être honnête.** Dis ce que tu n'as pas fait, pas pu vérifier ou pas
  compris. Un échec signalé vaut mieux qu'une réussite inventée.
- **Ne pas deviner.** Face à une ambiguïté qui change le résultat, demande.
  Pour un détail sans conséquence, choisis l'option raisonnable et signale-la.
- **Laisser une trace.** Explique ce que tu as changé et pourquoi, pour que
  la personne suivante, humaine ou agent, puisse reprendre ton travail.

## 2. Le projet

<!--
CONSIGNES POUR L'AGENT : remplis cette section à la mise en place, puis supprime ce commentaire.

Si le projet existe déjà :
- Déduis chaque champ du dépôt : README, fichiers de dépendances
  (package.json, pyproject.toml, Cargo.toml, go.mod…), Makefile, scripts,
  configuration de la CI.
- Exécute chaque commande avant de l'écrire ici, pour vérifier qu'elle marche.
- Ce que tu ne peux pas déduire avec certitude : écris « À confirmer » et
  demande à l'humain.

Si le projet n'existe pas encore :
- Demande à l'humain le but du projet, son public et la stack souhaitée.
- Remplis avec ses réponses. Les commandes restent « À définir » jusqu'à
  l'initialisation du projet ; mets-les à jour à ce moment-là.

Dans les deux cas, reste court : le détail va dans docs/context/.
-->

**But** : [Une phrase : ce que fait le projet et pour qui.]
> Exemple : Une API qui permet aux clubs de sport de gérer les réservations de leurs terrains.

**Stack** : [Langage, framework, base de données, hébergement.]
> Exemple : TypeScript, Node.js, Fastify, PostgreSQL, déployé sur Fly.io.

**Commandes**

| Action | Commande |
| --- | --- |
| Installer | [ex. `npm install`] |
| Lancer en local | [ex. `npm run dev`] |
| Tester | [ex. `npm test`] |
| Tout vérifier (lint, types, tests) | [ex. `npm run check`] |

**Organisation du code** : [Les 3 à 5 dossiers principaux et leur rôle.]
> Exemple : `src/routes/` (endpoints HTTP), `src/domain/` (logique métier), `src/db/` (accès aux données).

**État** : [Prototype, en développement ou en production.]

## 3. Comment tu travailles une tâche

Adapte l'effort à la taille de la tâche :

| Taille | Exemple | Plan |
| --- | --- | --- |
| Petite | Corriger un bug localisé, renommer, ajuster un texte | Aucun plan écrit |
| Moyenne | Ajouter un comportement sur quelques fichiers | Plan court annoncé avant de commencer |
| Grande | Nouvelle fonctionnalité, refonte, changement d'architecture | Plan écrit, validé par l'humain avant de coder |

Dans tous les cas, suis ce cycle :

1. **Comprendre.** Reformule l'objectif en une phrase. Lis la demande en
   entier, le code concerné et les règles qui s'appliquent. Repère ce qui
   est hors périmètre.
2. **Planifier.** Selon la taille : les étapes, les fichiers touchés et la
   façon de vérifier le résultat.
3. **Réaliser.** Une étape à la fois. Après chaque étape importante, vérifie
   que rien n'est cassé avant de passer à la suivante.
4. **Vérifier.** Lance la commande « Tout vérifier » de la section 2. Teste
   le comportement réel, pas seulement que le code compile. Relis ton diff
   comme le ferait un relecteur.
5. **Livrer.** Résume ce qui a été fait, comment tu l'as vérifié, et ce qui
   reste à faire ou n'a pas pu être vérifié.

Si tu découvres en cours de route que la tâche est plus grosse ou différente
de ce qui était prévu, arrête-toi. Explique ce que tu as trouvé et ajuste le
plan avec l'humain, plutôt que d'élargir le périmètre seul.
