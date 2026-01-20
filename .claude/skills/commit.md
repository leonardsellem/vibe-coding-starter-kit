# Commit (Message en français)

Committe les changements avec un message structuré en français suivant les conventions.

## Quand utiliser

- Après avoir terminé une modification
- Quand tu veux sauvegarder ton travail
- Avant de passer à une autre tâche

## Pourquoi c'est important

Des bons messages de commit :
- Permettent de comprendre l'historique du projet
- Facilitent le debugging (git blame)
- Aident les autres développeurs (et ton futur toi)

## Instructions

Quand l'utilisateur invoque `/commit`, suivre ce processus :

### 1. Analyser l'état actuel

```bash
git status          # Voir les fichiers modifiés
git diff --stat     # Vue résumée des changements
git log --oneline -3  # Contexte des derniers commits
```

### 2. Stager tous les changements

```bash
git add -A
```

### 3. Analyser le diff complet

```bash
git diff --cached   # Voir exactement ce qui sera commité
```

### 4. Générer le message

Analyser le diff et proposer un message selon cette convention :

```
type(scope): description courte en impératif présent

[corps optionnel avec contexte et motivations]
```

**Types disponibles :**

| Type | Quand l'utiliser |
|------|------------------|
| `feat` | Nouvelle fonctionnalité pour l'utilisateur |
| `fix` | Correction d'un bug |
| `refactor` | Changement de code sans modifier le comportement |
| `perf` | Amélioration de performance |
| `docs` | Changement de documentation uniquement |
| `style` | Formatage, espaces, point-virgules (pas de logique) |
| `test` | Ajout ou correction de tests |
| `chore` | Build, config, dépendances |
| `ci` | Configuration CI/CD |

### 5. Valider avec l'utilisateur

Proposer 1-2 options de message via `AskUserQuestion`, puis exécuter le commit.

### 6. Confirmer

Afficher le résultat avec le hash du commit.

## Règles de rédaction

| Règle | Bon exemple | Mauvais exemple |
|-------|-------------|-----------------|
| Impératif présent | "ajoute la validation" | "ajouté la validation" |
| Pas de point final | "corrige le bug login" | "corrige le bug login." |
| Max 72 caractères | "feat(auth): ajoute OAuth" | "feat(authentification): ajoute la fonctionnalité de connexion OAuth Google avec refresh token" |
| Scope précis | "fix(parser)" | "fix(code)" |

## Exemples complets

### Nouvelle fonctionnalité
```
feat(panier): ajoute le calcul automatique des frais de port

Les frais sont calculés selon le poids total et la zone de livraison.
Utilise l'API Colissimo pour les tarifs en temps réel.
```

### Correction de bug
```
fix(checkout): corrige le crash sur panier vide

Le bouton "Payer" était actif même sans articles.
Ajoute une vérification avant de lancer le paiement.
```

### Refactoring
```
refactor(api): extrait la validation dans un middleware

Réduit la duplication dans les routes /users et /products.
Pas de changement de comportement.
```

### Breaking change
```
feat(api): change le format de réponse des erreurs

BREAKING CHANGE: Les erreurs retournent maintenant
{ "error": { "code": "...", "message": "..." } }
au lieu de { "message": "..." }
```

## Ce qui n'est PAS un bon message

| Mauvais | Pourquoi |
|---------|----------|
| "fix" | Trop vague, pas de contexte |
| "WIP" | Ne décrit rien |
| "changements divers" | Inutile pour l'historique |
| "ça marche maintenant" | Pas professionnel |
| "Update index.js" | Ne dit pas QUOI a changé |

## Astuce

Si tu ne sais pas quel type utiliser, pose-toi la question :
- "Ça ajoute quelque chose de nouveau ?" → `feat`
- "Ça corrige un problème ?" → `fix`
- "Ça change la structure sans changer le comportement ?" → `refactor`
- "Ça touche uniquement la doc ?" → `docs`
