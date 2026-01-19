---
description: Vérifie qu'une tâche est terminée avant commit
allowed-tools: Bash(npm test:*), Bash(npm run lint:*), Bash(git diff:*), Bash(git status:*), Bash(yarn:*), Bash(pnpm:*), Bash(bun:*)
---

Exécute la checklist de vérification avant de déclarer une tâche terminée.

## Processus INTERACTIF

**IMPORTANT** : Utiliser `AskUserQuestion` pour adapter les commandes au projet et confirmer chaque étape.

### Étape 0 : Identifier les commandes du projet

Si les commandes de test/lint ne sont pas connues :

```json
{
  "questions": [{
    "question": "Quel gestionnaire de paquets utilises-tu ?",
    "header": "Package",
    "multiSelect": false,
    "options": [
      {"label": "npm", "description": "npm test / npm run lint"},
      {"label": "yarn", "description": "yarn test / yarn lint"},
      {"label": "pnpm", "description": "pnpm test / pnpm lint"},
      {"label": "bun", "description": "bun test / bun lint"}
    ]
  }]
}
```

### Étape 1 : Tests automatisés

Exécuter la commande de test, puis demander confirmation :

```json
{
  "questions": [{
    "question": "Les tests passent-ils tous ? (Je viens de les exécuter)",
    "header": "Tests",
    "multiSelect": false,
    "options": [
      {"label": "Oui, tous OK", "description": "Continuer la vérification"},
      {"label": "Échecs", "description": "Il faut corriger avant de continuer"},
      {"label": "Pas de tests", "description": "Ce projet n'a pas de tests automatisés"}
    ]
  }]
}
```

### Étape 2 : Lint / Formatage

Exécuter lint, puis demander confirmation :

```json
{
  "questions": [{
    "question": "Le lint passe-t-il sans erreur ?",
    "header": "Lint",
    "multiSelect": false,
    "options": [
      {"label": "Oui, propre", "description": "Aucune erreur de lint"},
      {"label": "Warnings only", "description": "Des warnings mais pas bloquant"},
      {"label": "Erreurs", "description": "Il faut corriger"},
      {"label": "Pas de lint", "description": "Ce projet n'a pas de linter configuré"}
    ]
  }]
}
```

### Étape 3 : Vérification manuelle

```json
{
  "questions": [{
    "question": "As-tu testé manuellement le comportement ?",
    "header": "Manuel",
    "multiSelect": true,
    "options": [
      {"label": "Happy path OK", "description": "Le cas normal fonctionne"},
      {"label": "Erreurs gérées", "description": "Les cas d'erreur sont propres"},
      {"label": "Edge cases OK", "description": "Les cas limites fonctionnent"},
      {"label": "Pas testé", "description": "Je n'ai pas pu tester manuellement"}
    ]
  }]
}
```

### Étape 4 : Review du diff

Afficher `git diff`, puis demander :

```json
{
  "questions": [{
    "question": "Le diff est-il propre ?",
    "header": "Diff",
    "multiSelect": true,
    "options": [
      {"label": "Fichiers attendus", "description": "Seuls les fichiers voulus sont modifiés"},
      {"label": "Pas de debug", "description": "Pas de console.log ou print oublié"},
      {"label": "Pas de secrets", "description": "Pas de clés API ou mots de passe"},
      {"label": "Problème détecté", "description": "Il y a quelque chose à nettoyer"}
    ]
  }]
}
```

### Étape 5 : Specs satisfaites

```json
{
  "questions": [{
    "question": "Les specs initiales sont-elles satisfaites ?",
    "header": "Specs",
    "multiSelect": false,
    "options": [
      {"label": "Oui, tout OK", "description": "Les requirements sont implémentés"},
      {"label": "Partiellement", "description": "Il manque des éléments"},
      {"label": "Non-goals respectés", "description": "Je n'ai pas dépassé le scope"}
    ]
  }]
}
```

### Étape finale : Résumé et action

```json
{
  "questions": [{
    "question": "Que veux-tu faire maintenant ?",
    "header": "Action",
    "multiSelect": false,
    "options": [
      {"label": "Commit", "description": "Tout est bon, prépare le commit"},
      {"label": "Corriger d'abord", "description": "Il y a des choses à fixer"},
      {"label": "Voir le résumé", "description": "Affiche-moi le rapport de vérification"}
    ]
  }]
}
```

## Output

Après toutes les vérifications, résumer :

```markdown
## Vérification complète

- [x] Tests : X passed, 0 failed
- [x] Lint : aucune erreur
- [x] Manuel : happy path OK, erreurs OK
- [x] Diff : N fichiers, pas de secrets
- [x] Specs : REQ-1 et REQ-2 satisfaits

**Prêt à commit.**
```

## Règle absolue

> **Ne jamais dire "c'est fait" sans avoir exécuté cette checklist ET obtenu confirmation à chaque étape.**
