# Sécurité

## Secrets

- **Les secrets viennent de l'environnement** (variables d'environnement,
  gestionnaire de secrets), jamais du code.
- **`.env` est dans `.gitignore`.** Le dépôt contient seulement un
  `.env.example` avec des valeurs fictives, pour documenter les variables
  attendues.
- **Un secret n'apparaît nulle part en clair** : ni dans les logs, ni dans les
  messages d'erreur, ni dans les tests.
- **Si un secret a été commité, arrête-toi et préviens l'humain tout de
  suite.** Le retirer du code ne suffit pas : il reste dans l'historique et
  doit être révoqué.

## Entrées externes

Tout ce qui vient de l'extérieur est suspect : saisie utilisateur, requête
HTTP, fichier importé, réponse d'une API, variable d'environnement.

- **Valide à l'entrée** : type, format, taille. Accepte ce qui est attendu
  plutôt que de chercher à refuser ce qui est dangereux.
- **Ne construis jamais une requête, une commande ou un chemin par
  concaténation.** Utilise des requêtes paramétrées, évite de passer une entrée
  à un shell, et vérifie qu'un chemin de fichier reste dans le dossier prévu.
- **Échappe à la sortie** : laisse le framework échapper le HTML, plutôt que
  d'insérer une entrée telle quelle dans une page.

## Authentification et permissions

- **Vérifie les droits côté serveur, à chaque action.** Masquer un bouton
  dans l'interface ne protège rien.
- **Refuse par défaut** : un accès non prévu explicitement est interdit.
- **N'invente pas de cryptographie.** Pour les mots de passe, les tokens et le
  chiffrement, utilise les bibliothèques éprouvées de l'écosystème.
- **Les messages d'erreur ne renseignent pas l'attaquant.** Par exemple,
  « identifiants invalides » plutôt que « cet email n'existe pas ».

## Données personnelles

- **Collecte le minimum** nécessaire à la fonctionnalité.
- **Jamais dans les logs.**
- **Données de test fictives**, jamais copiées depuis la production.

## Dépendances

- **Vérifie que le paquet existe vraiment** et que son nom est exact, avant de
  l'installer. Un nom inventé ou mal orthographié peut correspondre à un paquet
  malveillant publié exprès.
- **Préfère une bibliothèque connue et maintenue** à un paquet obscur.
- **Le fichier de verrouillage des versions est commité** (`package-lock.json`,
  `poetry.lock`, `Cargo.lock`…).
- **Si le projet a un outil d'audit** (`npm audit`, `pip-audit`…), lance-le
  après un ajout.

## Si tu trouves une faille

Ne la corrige pas en douce et ne la décris pas dans une issue publique.
Préviens l'humain directement : il décidera comment la traiter.
