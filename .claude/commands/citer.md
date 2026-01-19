---
description: Structure un prompt avec le format CITER
argument-hint: [demande brute]
---

Aide l'utilisateur à structurer sa demande avec le format CITER.

## Les 5 éléments

| Lettre | Élément | Question |
|--------|---------|----------|
| **C** | Context | Qu'est-ce qui existe déjà ? |
| **I** | Intent | Pourquoi je fais ça ? |
| **T** | Task | Quoi exactement ? |
| **E** | Expected | À quoi ressemble le succès ? |
| **R** | Restrictions | Quoi NE PAS faire ? |

## Processus INTERACTIF

**IMPORTANT** : Utiliser l'outil `AskUserQuestion` pour collecter chaque élément. Ne pas poser toutes les questions en même temps.

### Étape 1 : Collecter C (Context)

Si $ARGUMENTS est vague, commencer par le contexte :

```json
{
  "questions": [{
    "question": "Quel est le contexte actuel ? (fichiers existants, état du projet)",
    "header": "Context",
    "multiSelect": false,
    "options": [
      {"label": "Nouveau projet", "description": "Partir de zéro, rien n'existe encore"},
      {"label": "Fichier existant", "description": "Je dois modifier du code existant"},
      {"label": "Bug à corriger", "description": "Quelque chose ne fonctionne pas"},
      {"label": "Refactoring", "description": "Le code marche mais doit être amélioré"}
    ]
  }]
}
```

### Étape 2 : Collecter I (Intent)

```json
{
  "questions": [{
    "question": "Quel est l'objectif business ou technique derrière cette demande ?",
    "header": "Intent",
    "multiSelect": false,
    "options": [
      {"label": "Nouvelle feature", "description": "Ajouter une capacité pour les utilisateurs"},
      {"label": "Fix bug", "description": "Corriger un comportement incorrect"},
      {"label": "Améliorer perf", "description": "Rendre plus rapide ou efficace"},
      {"label": "Réduire dette", "description": "Simplifier ou nettoyer le code"}
    ]
  }]
}
```

### Étape 3 : Collecter T (Task)

```json
{
  "questions": [{
    "question": "Quelle action spécifique veux-tu que je réalise ?",
    "header": "Task",
    "multiSelect": false,
    "options": [
      {"label": "Créer fichier", "description": "Écrire du nouveau code"},
      {"label": "Modifier existant", "description": "Éditer du code en place"},
      {"label": "Analyser", "description": "Comprendre et expliquer"},
      {"label": "Debugger", "description": "Trouver et corriger un problème"}
    ]
  }]
}
```

### Étape 4 : Collecter E (Expected)

```json
{
  "questions": [{
    "question": "Comment sauras-tu que c'est réussi ?",
    "header": "Expected",
    "multiSelect": true,
    "options": [
      {"label": "Tests passent", "description": "Les tests automatisés valident"},
      {"label": "UI visible", "description": "Je peux voir le résultat dans l'interface"},
      {"label": "Pas d'erreur", "description": "Plus de message d'erreur"},
      {"label": "Comportement OK", "description": "La fonctionnalité fait ce qu'elle doit"}
    ]
  }]
}
```

### Étape 5 : Collecter R (Restrictions)

```json
{
  "questions": [{
    "question": "Qu'est-ce que tu veux que j'évite absolument ?",
    "header": "Restrictions",
    "multiSelect": true,
    "options": [
      {"label": "Pas de nouvelle dépendance", "description": "Utiliser ce qui existe déjà"},
      {"label": "Ne pas toucher à X", "description": "Certains fichiers sont hors scope"},
      {"label": "Garder rétrocompat", "description": "Ne rien casser pour les utilisateurs actuels"},
      {"label": "Pas de refacto", "description": "Faire le minimum, pas de cleanup"}
    ]
  }]
}
```

### Étape 6 : Synthèse et validation

Reformuler en prompt structuré puis demander validation :

```
**Context :** [ce qui existe]
**Intent :** [pourquoi]
**Task :** [quoi faire]
**Expected :** [critères de succès]
**Restrictions :** [ce qu'il ne faut pas faire]
```

```json
{
  "questions": [{
    "question": "Ce prompt structuré te convient ? Je peux commencer ?",
    "header": "Validation",
    "multiSelect": false,
    "options": [
      {"label": "Oui, c'est bon", "description": "Lance-toi"},
      {"label": "Ajuster", "description": "Je veux modifier un élément"},
      {"label": "Recommencer", "description": "C'est pas du tout ça"}
    ]
  }]
}
```

## Règle importante

**Ne jamais poser les 5 questions CITER d'un coup.** Procéder élément par élément, attendre chaque réponse avant de continuer.

$ARGUMENTS
