---
description: Documente le travail en cours (notes de session)
---

Met à jour les notes de session pour assurer la continuité du travail.

## Format de notes

```markdown
## Session [DATE]

**COMPLETED:**
- [Ce qui est terminé, avec chemins de fichiers]

**IN_PROGRESS:**
- [État actuel + prochaine étape immédiate]

**BLOCKERS:**
- [Ce qui bloque, si applicable]

**DECISIONS:**
- [Choix faits et pourquoi]

**DISCOVERED:**
- [Découvertes inattendues, nouveaux problèmes]
```

## Processus INTERACTIF

**IMPORTANT** : Utiliser l'outil `AskUserQuestion` pour collecter les informations de manière guidée.

### Étape 1 : État actuel

```json
{
  "questions": [{
    "question": "Où en es-tu dans ton travail actuel ?",
    "header": "État",
    "multiSelect": false,
    "options": [
      {"label": "En plein milieu", "description": "Je code activement, je veux sauvegarder l'état"},
      {"label": "Tâche terminée", "description": "J'ai fini quelque chose, je veux documenter"},
      {"label": "Bloqué", "description": "Je suis coincé sur un problème"},
      {"label": "Fin de session", "description": "Je m'arrête, je veux un handoff propre"}
    ]
  }]
}
```

### Étape 2 : Collecter COMPLETED

```json
{
  "questions": [{
    "question": "Qu'est-ce qui a été terminé depuis la dernière fois ?",
    "header": "Complété",
    "multiSelect": true,
    "options": [
      {"label": "Nouveau fichier créé", "description": "J'ai créé un ou plusieurs fichiers"},
      {"label": "Code modifié", "description": "J'ai édité du code existant"},
      {"label": "Tests écrits", "description": "J'ai ajouté des tests"},
      {"label": "Bug corrigé", "description": "J'ai résolu un problème"}
    ]
  }]
}
```

Après la sélection, demander les détails spécifiques (fichiers, lignes).

### Étape 3 : Collecter IN_PROGRESS

```json
{
  "questions": [{
    "question": "Sur quoi travailles-tu en ce moment ?",
    "header": "En cours",
    "multiSelect": false,
    "options": [
      {"label": "Implémentation", "description": "Je code une fonctionnalité"},
      {"label": "Tests", "description": "J'écris ou debug des tests"},
      {"label": "Refactoring", "description": "Je réorganise du code"},
      {"label": "Debugging", "description": "Je cherche un bug"}
    ]
  }]
}
```

### Étape 4 : Collecter BLOCKERS (si applicable)

```json
{
  "questions": [{
    "question": "Y a-t-il quelque chose qui te bloque ?",
    "header": "Bloqueurs",
    "multiSelect": true,
    "options": [
      {"label": "Aucun blocage", "description": "Tout roule"},
      {"label": "Besoin d'info", "description": "Il me manque une clarification"},
      {"label": "Dépendance externe", "description": "J'attends quelqu'un d'autre"},
      {"label": "Bug incompris", "description": "Je ne comprends pas pourquoi ça ne marche pas"}
    ]
  }]
}
```

### Étape 5 : Collecter DECISIONS

```json
{
  "questions": [{
    "question": "As-tu fait des choix techniques importants à noter ?",
    "header": "Décisions",
    "multiSelect": true,
    "options": [
      {"label": "Choix d'architecture", "description": "Structure de fichiers, patterns utilisés"},
      {"label": "Choix de librairie", "description": "Utilisation d'une dépendance spécifique"},
      {"label": "Choix UX", "description": "Comportement utilisateur décidé"},
      {"label": "Aucune décision", "description": "Pas de choix majeur à noter"}
    ]
  }]
}
```

### Étape 6 : Collecter DISCOVERED

```json
{
  "questions": [{
    "question": "As-tu découvert quelque chose d'inattendu ?",
    "header": "Découvertes",
    "multiSelect": true,
    "options": [
      {"label": "Nouveau bug", "description": "J'ai trouvé un problème non prévu"},
      {"label": "Code existant utile", "description": "Il y avait déjà quelque chose de réutilisable"},
      {"label": "Travail supplémentaire", "description": "Il y a plus à faire que prévu"},
      {"label": "Rien de spécial", "description": "Pas de surprise"}
    ]
  }]
}
```

### Étape 7 : Où sauvegarder

```json
{
  "questions": [{
    "question": "Où veux-tu sauvegarder ces notes ?",
    "header": "Destination",
    "multiSelect": false,
    "options": [
      {"label": "NOTES.md", "description": "Fichier NOTES.md à la racine du projet"},
      {"label": "Afficher seulement", "description": "Juste me montrer, je copie moi-même"},
      {"label": "Ticket/Issue", "description": "Format pour coller dans un ticket"}
    ]
  }]
}
```

## Conseils
- Être spécifique : "LoginForm.tsx:45" > "le formulaire"
- Toujours inclure le NEXT dans IN_PROGRESS
- Ne pas filtrer DISCOVERED, même les petites choses
