# orbit-agent

Un harness de projet pour les agents de code : les règles, le cycle de travail
et le contexte dont un agent a besoin pour travailler dans un projet comme un
développeur de l'équipe.

Il ne dépend d'aucun langage ni d'aucun outil. Ce sont des fichiers Markdown,
à copier dans n'importe quel projet.

## Principe

- **`AGENTS.md`** est court et lu à chaque session : la posture, le projet, le
  cycle de travail, la Definition of Done, les règles d'or et quand
  s'arrêter.
- **`docs/`** contient le détail, lu seulement quand il sert : `AGENTS.md`
  indique quel fichier lire et à quel moment.
- **Tout changement part d'une issue** petite et validée, et aboutit à une PR
  liée à cette issue. Une issue naît avec le label `needs-validation`, et la
  CI refuse la PR tant que le valideur ne l'a pas retiré.

## Contenu

```
AGENTS.md                         les règles lues à chaque session
CLAUDE.md                         pointe vers AGENTS.md, pour Claude Code
.github/
├── ISSUE_TEMPLATE/issue.md       modèle d'issue
├── pull_request_template.md      modèle de PR
└── workflows/
    ├── orbit-policy.yml          vérifie les règles de la PR (check policy)
    └── orbit.yml                 vérifie le code de la PR (check code)
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

## Vérifications automatiques

Les règles critiques ne reposent pas seulement sur la bonne volonté de
l'agent : deux checks les vérifient sur chaque PR.

- **`policy`** (`.github/workflows/orbit-policy.yml`) :
  - la description contient `Closes #N`, et l'issue est ouverte et validée
    (ni `needs-validation` ni `blocked`). Les PR de Dependabot et Renovate en
    sont dispensées (liste `EXEMPT_BOTS`), pas des vérifications suivantes :
    pour Dependabot, régler `commit-message.prefix` (par exemple `build`) ;
  - le titre et les commits suivent Conventional Commits ;
  - une PR qui touche `AGENTS.md`, `docs/rules/` ou `.github/` porte le label
    `rules-change`, posé par l'humain.
- **`code`** (`.github/workflows/orbit.yml`) :
  - aucun secret n'est présent dans les commits (gitleaks, appelé
    directement : l'action `gitleaks/gitleaks-action` exige une licence
    `GITLEAKS_LICENSE` pour les dépôts d'organisation) ;
  - la commande `check` du projet passe (à compléter à l'installation).

`policy` tourne sur `pull_request_target` : c'est la version de la main branch
qui s'exécute, sans checkout du code de la PR. Une PR ne peut donc pas
affaiblir ces vérifications. `code` a besoin du code de la PR : il tourne sur
`pull_request`, dans la version présente dans la PR.

Avec la protection de la main branch, une PR qui ne passe pas ces deux checks
ne peut pas être mergée.

**Limites**

- `policy` ne prend effet qu'une fois `orbit-policy.yml` sur la main branch :
  la PR qui l'ajoute n'est pas vérifiée par lui.
- Une PR peut modifier `orbit.yml`, donc le check `code`. C'est pourquoi
  `.github/` est protégé comme les règles (label `rules-change`).
- `rules-change` est un signal, pas un verrou : un agent qui utilise le compte
  GitHub de l'humain peut le poser. La règle d'or 9 le lui interdit, et la
  review de l'humain reste nécessaire.
- Retirer un label de l'issue ne relance pas `policy` : relancer le job
  depuis la PR.

## Installation

### Avec un agent (recommandé)

Dans le projet, donner ce prompt à l'agent :

> Installe le harness orbit-agent dans ce projet. Clone
> `https://github.com/tabiitha/orbit-agent.git` dans un dossier temporaire, lis
> la section « Pour les agents » de son `README.md` et suis-la.

Ça marche pour un projet existant comme pour un projet qui démarre : l'agent
s'adapte et pose des questions pour ce qu'il ne peut pas déduire.

### À la main

1. Copier `AGENTS.md`, `CLAUDE.md`, `docs/` et `.github/` à la racine du projet, sans
   écraser les fichiers existants.
2. Créer les labels `needs-validation`, `blocked` et `rules-change` sur le
   dépôt GitHub.
3. Remplir la section 2 d'`AGENTS.md` et `docs/context/architecture.md`, en
   suivant les consignes en commentaire.
4. Compléter l'étape `check` de `.github/workflows/orbit.yml`.
5. Protéger la main branch (*Settings → Rules → Rulesets*) : exiger
   une PR, exiger que les checks `policy` et `code` passent, interdire le
   `push --force`.
6. Relire et commiter.

`CLAUDE.md` contient seulement `@AGENTS.md`, pour que Claude Code lise les
mêmes règles. Si un autre outil ne lit pas `AGENTS.md` de lui-même, créer de la
même façon le fichier qu'il attend.

## Adapter à un projet

- **`AGENTS.md`, `docs/rules/` et `.github/`** sont les règles et leurs
  garde-fous : seul l'humain les modifie, et seul lui pose le label
  `rules-change` qui autorise une PR à les toucher.
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
2. **Travaille sur une branche** `chore/install-orbit-agent`. Si le projet
   n'est pas encore un dépôt git, demande à l'humain avant de lancer `git init`.
3. **Copie `AGENTS.md`, `CLAUDE.md`, `docs/` et `.github/`** depuis le clone.
   Ne copie ni ce `README.md` ni `LICENSE`. N'écrase aucun fichier existant :
   en cas de conflit, montre les différences à l'humain et propose une fusion.
4. **Crée les labels** utilisés par le graphe et la CI :
   ```bash
   gh label create needs-validation --description "Issue à valider avant de commencer"
   gh label create blocked --description "En attente d'une réponse de l'humain"
   gh label create rules-change --description "L'humain autorise cette PR à modifier les règles ou la CI"
   ```
   Si le projet n'est pas sur GitHub ou si `gh` n'est pas disponible,
   signale-le à l'humain.
5. **Remplis le contexte**, en suivant les consignes en commentaire de la
   section 2 d'`AGENTS.md` et de `docs/context/architecture.md` :
   - **Projet existant** : déduis tout du dépôt, exécute chaque commande avant
     de l'écrire, et marque `TBC` ce qui reste incertain.
   - **Nouveau projet** : demande à l'humain le but, le public et la stack.
     Les commandes restent `TBD` et l'architecture reste vide.

   Supprime chaque commentaire de consignes une fois sa partie remplie.
6. **Complète la CI.** Dans `.github/workflows/orbit.yml`, remplace l'étape
   `check` en commentaire par l'installation de la stack et la
   commande `check` de la section 2. Si les commandes sont encore
   `TBD`, laisse l'étape en commentaire et signale-le.
7. **Propose la protection de la main branch** : PR obligatoire, checks
   `policy` et `code` requis, `push --force` interdit. Ne l'active toi-même
   que si l'humain le demande. N'exige pas de review approuvée si les agents
   utilisent le compte GitHub de l'humain : il ne pourrait plus merger
   leurs PR.
8. **Signale les écarts avec l'existant.** Si le projet a déjà ses propres
   conventions (format de commit, nommage des branches, outils de test),
   présente-les à l'humain plutôt que de choisir entre elles et celles
   d'orbit-agent.
9. **Fais lire `AGENTS.md` à ton outil.** S'il ne le lit pas de lui-même, crée
   le fichier qu'il attend, en le faisant pointer vers `AGENTS.md` plutôt
   qu'en recopiant son contenu.
10. **Ne commite pas sur la main branch.** Termine par un résumé : les
   fichiers ajoutés, les conflits trouvés, les points `TBC` et les
   questions en attente. L'humain relit, puis décide de la suite.
11. **Supprime le clone temporaire.**

## Licence

MIT, voir [`LICENSE`](LICENSE).
