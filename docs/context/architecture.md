# Architecture

<!--
CONSIGNES POUR L'AGENT : remplis ce fichier à la mise en place, puis supprime ce commentaire.

Si le projet existe déjà : déduis chaque partie du code et de la configuration.
Ce que tu ne peux pas déduire avec certitude : écris `TBC` et
demande à l'humain.

Si le projet n'existe pas encore : laisse les parties vides et remplis-les au
fil des premières issues, quand l'architecture prend forme.

Reste court : ce fichier est une carte, pas une documentation complète. Mets-le
à jour dans la PR qui change l'architecture.
-->

## Vue d'ensemble

[Deux ou trois phrases : comment le projet est construit.]
> Exemple : Une API HTTP sans état. Chaque requête passe par une route, qui
> appelle la logique métier, qui lit ou écrit en base.

## Composants

| Dossier | Rôle |
| --- | --- |
| [ex. `src/routes/`] | [ex. Endpoints HTTP : validation des entrées, appel du domaine] |
| [ex. `src/domain/`] | [ex. Logique métier, sans accès direct à la base] |
| [ex. `src/db/`] | [ex. Accès aux données, migrations] |

## Flux principal

[Le chemin d'une action typique à travers les composants.]
> Exemple : requête HTTP → `routes/` (validation) → `domain/` (règles de
> réservation) → `db/` (écriture) → réponse.

## Services externes

[Base de données, API tierces, files de messages : ce que le projet appelle.]
> Exemple : PostgreSQL (données), Stripe (paiements), Resend (emails).

## Points sensibles

[Ce qui casse facilement ou surprend : à lire avant de toucher.]
> Exemple : Les créneaux sont stockés en UTC mais affichés dans le fuseau du
> club. Toute comparaison de dates passe par `domain/time.ts`.
