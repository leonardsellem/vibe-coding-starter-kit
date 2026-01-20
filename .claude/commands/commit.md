---
description: Commit les changements avec un message en français
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git diff:*), Bash(git log:*), Bash(git commit:*)
---

Aide l'utilisateur à committer ses changements avec un message structuré en français.

## Processus

### Étape 1 : Analyser l'état du repo

Exécuter ces commandes en parallèle :

```bash
git status
git diff --stat
git log --oneline -3
```

### Étape 2 : Stager les changements

```bash
git add -A
```

### Étape 3 : Analyser le diff

```bash
git diff --cached
```

### Étape 4 : Générer le message de commit

Analyser le diff et proposer un message selon la convention française.

**Format :**
```
type(scope): description courte (max 72 caractères)

[corps optionnel : contexte, motivations]
```

**Types usuels :**
| Type | Description |
|------|-------------|
| `feat` | nouvelle fonctionnalité |
| `fix` | correction de bug |
| `refactor` | refactorisation sans changement de comportement |
| `perf` | amélioration de performance |
| `docs` | documentation uniquement |
| `style` | formatage, espaces, point-virgules |
| `test` | ajout ou correction de tests |
| `chore` | tâches build, config, dépendances |
| `ci` | configuration CI/CD |

**Règles :**
- Scope entre parenthèses si pertinent (composant, module, fichier)
- Impératif présent : "ajoute" pas "ajouté" ou "ajouter"
- Pas de point final au résumé
- Corps séparé par ligne vide, wrappé à 72 caractères
- Breaking changes précédés de "BREAKING CHANGE:"

### Étape 5 : Proposer et valider

Afficher 1-2 propositions de message, puis demander :

```json
{
  "questions": [{
    "question": "Quel message de commit veux-tu utiliser ?",
    "header": "Message",
    "multiSelect": false,
    "options": [
      {"label": "Option 1", "description": "[premier message proposé]"},
      {"label": "Option 2", "description": "[deuxième message proposé]"},
      {"label": "Personnaliser", "description": "Je veux écrire mon propre message"}
    ]
  }]
}
```

### Étape 6 : Exécuter le commit

Si l'utilisateur choisit "Personnaliser", demander le message via `AskUserQuestion`.

Sinon, exécuter :

```bash
git commit -m "$(cat <<'EOF'
[message choisi]
EOF
)"
```

### Étape 7 : Confirmer

Afficher le résultat du commit et le hash.

## Exemples de messages en français

```
feat(auth): ajoute la connexion OAuth Google

fix(parser): corrige l'échappement des guillemets dans les strings JSON

refactor(api): extrait la validation dans un middleware dédié

perf(query): indexe la colonne user_id pour accélérer les recherches

docs(readme): met à jour les instructions d'installation

chore(deps): met à jour les dépendances npm

test(user): ajoute les tests unitaires pour le service utilisateur
```

## Si aucun changement

Si `git status` montre un working tree propre :

```json
{
  "questions": [{
    "question": "Pas de changement à committer. Que veux-tu faire ?",
    "header": "Action",
    "multiSelect": false,
    "options": [
      {"label": "Voir l'historique", "description": "Afficher les derniers commits"},
      {"label": "Rien", "description": "Terminer"}
    ]
  }]
}
```
