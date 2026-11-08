# orbit-agent

Un harness de projet pour les agents de code : les règles, le cycle de travail
et le contexte dont un agent a besoin pour travailler dans un projet comme un
développeur de l'équipe.

Il ne dépend d'aucun langage ni d'aucun outil. Ce sont des fichiers Markdown,
à copier dans n'importe quel projet.

## Principe

- **`AGENTS.md`** est court et lu à chaque session : la posture, le projet, le
  cycle de travail, la définition de « terminé », les règles d'or et quand
  s'arrêter.
- **`docs/`** contient le détail, lu seulement quand il sert : `AGENTS.md`
  indique quel fichier lire et à quel moment.
- **Tout changement part d'une issue** petite et validée, et aboutit à une PR
  liée à cette issue.

## Contenu

```
AGENTS.md                         les règles lues à chaque session
.github/
├── ISSUE_TEMPLATE/issue.md       modèle d'issue
└── pull_request_template.md      modèle de PR
docs/
├── workflow/
│   ├── issues.md                 rédiger, valider et découper une issue
│   ├── graph.md                  les étapes d'une issue et qui agit
│   └── loop.md                   recommencer après un échec, enchaîner, s'arrêter
├── rules/
│   ├── git.md                    branches, commits, PR
│   ├── tests.md                  quoi tester et comment
│   └── security.md               secrets, entrées, permissions, dépendances
└── context/                      propre à chaque projet, à remplir
    ├── architecture.md
    └── decisions/
```

## Installation

### Avec un agent (recommandé)

Dans le projet, donner ce prompt à l'agent :

> Installe le harness orbit-agent dans ce projet. Clone
> `https://github.com/tabiitha/orbit-agent.git` dans un dossier temporaire, lis
> la section « Pour les agents » de son `README.md` et suis-la.

Ça marche pour un projet existant comme pour un projet qui démarre : l'agent
s'adapte et pose des questions pour ce qu'il ne peut pas déduire.

### À la main

1. Copier `AGENTS.md`, `docs/` et `.github/` à la racine du projet, sans
   écraser les fichiers existants.
2. Créer les labels `needs-validation` et `blocked` sur le dépôt GitHub.
3. Remplir la section 2 d'`AGENTS.md` et `docs/context/architecture.md`, en
   suivant les consignes en commentaire.
4. Relire et commiter.

Si l'outil utilisé ne lit pas `AGENTS.md` de lui-même, le faire pointer vers ce
fichier. Par exemple, un `CLAUDE.md` qui contient seulement `@AGENTS.md`.

## Adapter à un projet

- **`AGENTS.md` et `docs/rules/`** sont les règles : seul l'humain les modifie.
- **La section 2 d'`AGENTS.md` et `docs/context/`** décrivent le projet :
  l'agent les tient à jour au fil des changements.
- **Garder `AGENTS.md` court.** Une règle qui ne sert pas à chaque tâche va
  dans `docs/`, avec une ligne dans le tableau « Où trouver quoi ».

## Pour les agents

Si on t'a demandé d'installer orbit-agent dans un projet, suis ces étapes.

1. **Regarde le projet.** Détermine s'il existe déjà (code, historique git) ou
   s'il démarre. Repère les fichiers qui entreraient en conflit : `AGENTS.md`,
   `CLAUDE.md` ou équivalent, `docs/`, `.github/ISSUE_TEMPLATE/`,
   `.github/pull_request_template.md`.
2. **Travaille sur une branche** `chore/installe-orbit-agent`. Si le projet
   n'est pas encore un dépôt git, demande à l'humain avant de lancer `git init`.
3. **Copie `AGENTS.md`, `docs/` et `.github/`** depuis le clone. Ne copie pas
   ce `README.md`. N'écrase aucun fichier existant : en cas de conflit, montre
   les différences à l'humain et propose une fusion.
4. **Crée les labels** utilisés par le graphe :
   ```bash
   gh label create needs-validation --description "Issue à valider avant de commencer"
   gh label create blocked --description "En attente d'une réponse de l'humain"
   ```
   Si le projet n'est pas sur GitHub ou si `gh` n'est pas disponible,
   signale-le à l'humain.
5. **Remplis le contexte**, en suivant les consignes en commentaire de la
   section 2 d'`AGENTS.md` et de `docs/context/architecture.md` :
   - **Projet existant** : déduis tout du dépôt, exécute chaque commande avant
     de l'écrire, et marque « À confirmer » ce qui reste incertain.
   - **Nouveau projet** : demande à l'humain le but, le public et la stack.
     Les commandes restent « À définir » et l'architecture reste vide.

   Supprime chaque commentaire de consignes une fois sa partie remplie.
6. **Signale les écarts avec l'existant.** Si le projet a déjà ses propres
   conventions (format de commit, nommage des branches, outils de test),
   présente-les à l'humain plutôt que de choisir entre elles et celles
   d'orbit-agent.
7. **Fais lire `AGENTS.md` à ton outil.** S'il ne le lit pas de lui-même, crée
   le fichier qu'il attend, en le faisant pointer vers `AGENTS.md` plutôt
   qu'en recopiant son contenu.
8. **Ne commite pas sur la branche principale.** Termine par un résumé : les
   fichiers ajoutés, les conflits trouvés, les points « À confirmer » et les
   questions en attente. L'humain relit, puis décide de la suite.
9. **Supprime le clone temporaire.**
