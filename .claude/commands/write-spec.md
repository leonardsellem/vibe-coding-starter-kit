---
description: Transforme une idée en specs claires (GIVEN/WHEN/THEN)
argument-hint: [idée ou fonctionnalité]
---

Aide l'utilisateur à transformer son idée en specs claires et vérifiables.

## Processus INTERACTIF

**IMPORTANT** : Utiliser l'outil `AskUserQuestion` pour chaque étape de clarification au lieu de lister les questions.

### Étape 1 : Clarifier l'idée

Si $ARGUMENTS est vide ou vague, utiliser AskUserQuestion :

```json
{
  "questions": [{
    "question": "Quel problème veux-tu résoudre avec cette fonctionnalité ?",
    "header": "Problème",
    "multiSelect": false,
    "options": [
      {"label": "Bug à corriger", "description": "Quelque chose ne fonctionne pas comme prévu"},
      {"label": "Nouvelle feature", "description": "Ajouter une capacité qui n'existe pas"},
      {"label": "Amélioration UX", "description": "Rendre quelque chose plus facile à utiliser"},
      {"label": "Performance", "description": "Rendre quelque chose plus rapide"}
    ]
  }]
}
```

### Étape 2 : Identifier l'utilisateur cible

```json
{
  "questions": [{
    "question": "Qui va utiliser cette fonctionnalité ?",
    "header": "Utilisateur",
    "multiSelect": false,
    "options": [
      {"label": "Utilisateur final", "description": "Le client/visiteur de l'app"},
      {"label": "Admin", "description": "Un administrateur du système"},
      {"label": "Développeur", "description": "Moi ou un autre dev"},
      {"label": "API/Machine", "description": "Un autre système automatisé"}
    ]
  }]
}
```

### Étape 3 : Définir le succès

```json
{
  "questions": [{
    "question": "Comment sauras-tu que c'est réussi ?",
    "header": "Succès",
    "multiSelect": true,
    "options": [
      {"label": "Message affiché", "description": "L'utilisateur voit une confirmation"},
      {"label": "Données sauvées", "description": "Les infos sont persistées en base"},
      {"label": "Action déclenchée", "description": "Une autre action se produit"},
      {"label": "Erreur évitée", "description": "Un cas d'erreur ne se produit plus"}
    ]
  }]
}
```

### Étape 4 : Rédiger les specs

Après les réponses, rédiger en GIVEN/WHEN/THEN :
```
REQ-1: [Nom descriptif]
- GIVEN [contexte ou état initial]
- WHEN [action de l'utilisateur]
- THEN [résultat observable et vérifiable]
```

### Étape 5 : Non-Goals

Utiliser AskUserQuestion pour valider les exclusions :
```json
{
  "questions": [{
    "question": "Que veux-tu explicitement exclure du scope ?",
    "header": "Non-Goals",
    "multiSelect": true,
    "options": [
      {"label": "Validation serveur", "description": "Sera fait plus tard côté backend"},
      {"label": "Mobile/responsive", "description": "Desktop only pour l'instant"},
      {"label": "Internationalisation", "description": "Français uniquement"},
      {"label": "Tests E2E", "description": "Tests unitaires suffisants pour l'instant"}
    ]
  }]
}
```

### Étape 6 : Valider

Utiliser AskUserQuestion pour confirmation finale :
```json
{
  "questions": [{
    "question": "Ces specs te conviennent ? Je peux commencer l'implémentation ?",
    "header": "Validation",
    "multiSelect": false,
    "options": [
      {"label": "Oui, c'est bon", "description": "Commence à coder"},
      {"label": "Ajuster les specs", "description": "Je veux modifier quelque chose"},
      {"label": "Ajouter des requirements", "description": "Il manque des cas"}
    ]
  }]
}
```

## Règles
- GIVEN = état de départ, pas d'action
- WHEN = une seule action déclencheuse
- THEN = résultat observable, pas d'implémentation
- **Ne jamais lister toutes les questions d'un coup** — poser une question, attendre la réponse, puis continuer

$ARGUMENTS
